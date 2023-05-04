# Build HikariNote

## Virtual environment
    python3 -m venv .hikarinote
    source .hikarinote/bin/activate

## Clone repository
    git clone https://github.com/hikarimusic/HikariNote.git
    cd HikariNote
    pip install -r requirements.txt

## Build and view
    make html
    firefox _build/html/index.html