# Church Sunday Setlist Playlist Automation

This project creates a new Spotify playlist for the next Sunday after today using a Google Sheet setlist.

The expected sheet shape is a dated block:

```text
05/31/2026 | Artist name | Name of the song | Link to the song | Vocalist/bandist | Comment
Intercession | Artist | Song | YouTube link | ignored | ignored
Praise 1 | Artist | Song | YouTube link | ignored | ignored
```

The script reads the target date block, searches Spotify in sheet order, creates a private playlist named like `Sunday Setlist 05-31-2026`, and adds confident matches.

## Setup

1. Create and activate a virtual environment.

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

2. Copy `.env.example` to `.env` and fill in the values.

```bash
copy .env.example .env
```

3. Google setup:

- Create a Google Cloud project.
- Enable the Google Sheets API.
- Create OAuth Client ID credentials for a Desktop app.
- Download the credentials file as `google_credentials.json`.
- Put it in this folder.

4. Spotify setup:

- Create an app in the Spotify Developer Dashboard.
- Add this redirect URI:

```text
http://127.0.0.1:8888/callback
```

- Put the Spotify Client ID and Client Secret in `.env`.

## Run

```bash
python create_sunday_playlist.py
```

The first run opens browser login flows for Google and Spotify. Later runs reuse local token files.

## Matching Behavior

- The sheet's `Artist name` and `Name of the song` are used first.
- YouTube and YouTube Music links are used to fetch the video title as an extra matching hint.
- TikTok links are logged as unsupported for now.
- Uncertain Spotify matches are skipped unless `ALLOW_UNCERTAIN_MATCHES=true`.
- Duplicate Spotify tracks are skipped after the first occurrence.

## Automation

On Windows, use Task Scheduler:

- Program: path to `python.exe`
- Arguments: full path to `create_sunday_playlist.py`
- Start in: this project folder

Schedule it weekly after the Google Sheet is normally updated.
