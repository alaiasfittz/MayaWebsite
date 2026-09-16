# Video files for the "Product storytelling" section

Drop the three source videos in this folder using these exact names:

| File            | Card                                        |
|-----------------|---------------------------------------------|
| `AB.mp4`        | Game-Day Style with Abercrombie x NFL       |
| `LMNT.mp4`      | Hydration Before Game Day                   |
| `Disney.mp4`    | Game-Day Style with Disney x Champion       |

Notes:

- The page also accepts the original upper-case extensions (`AB.MP4`,
  `Disney.MP4`) and falls back to `LMNT.MOV` if no `LMNT.mp4` is present.
- `.MOV` (QuickTime) does not play in Firefox and is unreliable in Chrome.
  Convert it for the web before publishing:
  `ffmpeg -i LMNT.MOV -c:v libx264 -crf 23 -preset slow -c:a aac -movflags +faststart LMNT.mp4`
- Run the same `-movflags +faststart` step on the MP4s so the browser can
  read metadata (and the poster frame) without downloading the whole file.
- Card duration and poster frame are read from the video at runtime, so no
  extra poster images are needed.
