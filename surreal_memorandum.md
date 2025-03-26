### Local connexion
Start an in-memory server for getting started without the environment's configuration complexity.
1. In a terminal window:
surreal start --log debug (--user root --pass root) / (--unauthenticated) memory
2. On another terminal window:
surreal sql --ns ns --db db [--user root --pass root] --pretty

### GUI
Reach https://www.surrealist.app/start to use the graphical user interface and click "Create connexion"
Try out the following settings for extra simplicity:
- protocol: WS
- hostname and port: localhost:8000 (as reported by the starting node event logged by debug-level setting)
- user: root
- pass word: root
- Click "Create" button then rendez-vous to the "Query" sub-menu to the left of the screen.