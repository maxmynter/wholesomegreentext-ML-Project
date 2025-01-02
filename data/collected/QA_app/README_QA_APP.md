# Quality assurance app

This is a (very scrappy) app for quality control of greentext extraction from posts on the `/r/wholesomegreentext` subreddit.

To start the frontend navigate into the `frontend` directory on a terminal and run:

```bash
npm run dev
```

For the backend, navigate into the `flask_backend` directory and run 

```bash
python app.py
```

Note: You must execute this from the directory indicated above for the filepaths to work out. Of course, you could program this in a more robust way, but this was a very quick and dirty solution to a task at hand, so i did not.


I have committed two images in the `data/collected/images` directory and their transcription in the `db.json` so you get an idea how the process worked.