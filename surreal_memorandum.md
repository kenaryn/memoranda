### Local connexion
Start an in-memory server for getting started without the environment's configuration complexity.
1. In a terminal window:
surreal start --log debug (--user root --pass root) / (--unauthenticated) memory
2. On another terminal window:
surreal sql --ns ns --db db [--user root --pass root] --pretty

### GUI
Reach https://www.surrealist.app/start to use the graphical user interface and click "Create connexion"