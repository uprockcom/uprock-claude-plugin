---
name: video
description: Search public video (YouTube, TikTok and other platforms) by meaning and read what happens at a moment, with time stamps. Use when the answer is shown or said in a video, or the user asks to find a video moment, a clip, or what a video says or shows at a given time.
---

# UpRock: Video search

Find the moment in public video where something is shown or said, then read that moment in full.

## How to do it

1. Call the `uprock` MCP tool **`video_search`** with:
   - `query`: what happens in the moment, in plain language (required). Describe the scene or what is said, not just keywords.
   - `unique_videos`: `true` (default) for one best moment per video; `false` for several moments from the same video.
   - `order_by_recency`: `true` to put the newest videos first.
   - `published_after` / `published_before`: unix timestamps (seconds, UTC) to limit by publish date.

2. Results come back best-first, up to 10. Each has `url`, `title`, `source` (e.g. `youtube`, `tiktok`), `published_at`, `score`, the moment's `t0` and `t1` in seconds, and a `transcript` excerpt when there is speech.
   - Judge relevance from the title and transcript, not the score alone. Search only covers indexed video, so if nothing fits, say so instead of stretching a weak match.

3. To read a moment, call **`video_moment`** with the hit's `url`, `t0` as `start_s` and `t1` as `end_s`.
   - Each moment has `t0_s`, `t1_s` and `text`: a description of what is on screen plus the spoken words.
   - It reads about 30 seconds per moment and up to `max_moments` (default 5, max 10) per call. To read further, call again with `start_s` = `next_start_s` while `has_more` is true.
   - If the video isn't indexed yet, the response says so and how long to wait.
   - Only set `include_images` (`hero`, `summary` or `full`) if you can view images; frames come back inline.

4. Answer with the video title and link, the time stamp (e.g. 1:15–1:45), and what happens there.

## Notes
- Moment text is generated from the video's frames and speech, so it can contain transcription errors. Quote it as what the video appears to say.
- Each search is billed per call; avoid running many near-identical searches.
