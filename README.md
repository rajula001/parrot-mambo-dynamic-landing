# Vision-Based Dynamic Landing on a Moving Platform (Parrot Mambo)

Autonomous landing of a Parrot Mambo drone onto a **moving RGB platform** using **MATLAB/Simulink + Stateflow**.  
The pipeline converts YUV→RGB, segments the red pad, latches detection, and triggers a **controlled descent** in a Stateflow decision chart. Full behavior is simulated with the Parrot Minidrone competition model and 3D visualization.

## System Overview
- **Image Processing**: RGB conversion → binary mask for red → pixel-count & centroid checks → `startDescendingFlag`.
- **Path Detection**: 3×3 grid (Left/Centre/Right × Top/Mid/Bot) to decide forward/turn behaviors.
- **Stateflow Control**: `Forward`, `TurnLeft`, `TurnRight`, `Backward`, `Land`; reduced vertical speed on detection for smooth touchdown.
- **Simulation**: Nonlinear multirotor body, sensors, and visualization blocks.

## Folder Structure
