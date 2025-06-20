# LLMChess

Integrating Language Models and Robotics for Chess
This repository contains the code for the project "Integrating Bayesian Optimization, Reinforcement Learning & Language Models in Robotics," specifically focusing on Chapter 3: Language Models in Robotics. The project demonstrates how to combine Large Language Models (LLMs) with a simulated robotic arm to perform the complex task of playing chess.

The system interprets high-level strategic instructions and natural language commands, validates them against the rules of chess and the physical environment, and executes the moves using a UR5e robotic arm in the PyBullet simulator.

Figure 3.5 from the report: The PyBullet chess environment setup.

🌟 Key Features
LLM-Powered Chess Agent: Utilizes gpt-4o-mini to generate strategic chess moves for one player (White).
Natural Language Interaction: Allows a human player (Black) to issue commands in natural language (e.g., "move my pawn from e7 to e5").
Robust Move Validation: Integrates the python-chess library to ensure all generated moves are legal according to the rules of chess.
Affordance Scoring: A novel pipeline grounds the LLM's suggestions in reality by checking if an action is physically possible and contextually valid before execution.
Robotic Execution: Translates validated chess moves into pick-and-place actions for a UR5e arm in a PyBullet simulation.
Vision-Based Manipulation (CLIPORT): An alternative pipeline that uses visual input (camera images) and text prompts to train a model for chess piece manipulation.
🏛️ System Architecture
The project's architecture is designed to bridge the gap between high-level language-based reasoning and low-level robotic control. The core pipeline is illustrated below, based on Figure 3.1 from the report.

A simplified flow diagram based on the report's system pipeline.

Input (Dual-Agent System):
White Player (LLM Decision-Maker): Receives the current board state (FEN), move history, and a strategic goal (e.g., "Play the London System"). It is prompted to return a move in a strict format: "Pick the [piece] and place it on the [square]".
Black Player (Human User): Issues a command in natural language. The system generates a list of possible pick-and-place actions.
Processing (Scoring & Validation):
LLM Decision-Maker: The generated command is parsed and validated against python-chess. If illegal, feedback is sent to the LLM for a retry.
Human User (SayCan-style):
LLM Scoring: The LLM scores how well each possible action matches the user's command.
Affordance Scoring: A critical filter checks each action for physical and logical validity:
Does the piece exist in the simulator?
Is the move legal for the current player (python-chess)?
Combined Score: The final score is LLM Score × Affordance Score. An invalid move gets a score of 0, effectively filtering it out.
Output (Robotic Execution):
The highest-scoring, valid action is selected.
The command is mapped to 3D coordinates in the workspace.
The UR5e arm executes the pick-and-place sequence in PyBullet.
