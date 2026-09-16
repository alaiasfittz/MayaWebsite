# Video assets for the "Product storytelling" section

## What is served

| File                  | Card                                  | Duration |
|-----------------------|---------------------------------------|----------|
| `AB.mp4`              | Game-Day Style with Abercrombie x NFL | 0:46     |
| `LMNT.mp4`            | Hydration Before Game Day             | 0:06     |
| `Disney.mp4`          | Game-Day Style with Disney x Champion | 1:27     |
| `EatInADay.mp4`       | What I Eat in a Day as an NFL Cheerleader | 0:29 |
| `Heat.mp4`            | How NFL Cheerleaders Prepare for the Heat | 0:10 |
| `Shopping.mp4`        | Shopping Local for Jaguars NFL Merch  | 0:08     |
| `DisneyTryOn.mp4`     | Disney x Champion NFL Apparel Try-On  | 0:40     |
| `posters/*.jpg`       | Card thumbnails, one frame per video  | —        |
| `photos/maya-portrait.jpg` | Product storytelling side photo  | —        |

The first three sit in "Product storytelling", the next two in "Sport and
wellness", the last two in "Brands and culture". All three sections use the
same card and modal player.

The page loads only the poster images up front; a video is fetched when its
card is clicked. Durations are printed in the HTML and re-synced from the file
itself the first time the video plays, so swapping a file keeps them honest.

## Where they came from

The originals in the repo root (`AB.MP4`, `LMNT.MOV`, `Disney.MP4`,
`Sports2.MP4` -> EatInADay, `sports.MP4` -> Heat, `shopping.MP4` ->
Shopping, `disneytryon.MP4` -> DisneyTryOn) are HEVC / H.265, which
Firefox cannot play and Chrome only plays on some machines. They were
transcoded to H.264 + AAC, which every current browser supports:

    ffmpeg -i ../AB.MP4     -vf scale=576:1024:flags=lanczos -c:v libx264 -profile:v high \
           -crf 26 -preset slow -pix_fmt yuv420p -c:a aac -b:a 112k -ac 2 \
           -movflags +faststart AB.mp4
    ffmpeg -i ../Disney.MP4 -vf scale=576:1024:flags=lanczos ... -crf 26 ... Disney.mp4
    ffmpeg -i ../LMNT.MOV   -vf scale=720:1280:flags=lanczos ... -crf 25 ... LMNT.mp4

`+faststart` puts the metadata at the front of the file so playback can begin
before the whole file has downloaded.

Posters were pulled from the transcoded files:

    ffmpeg -ss 4   -i AB.mp4     -frames:v 1 -q:v 3 posters/AB.jpg
    ffmpeg -ss 1.4 -i LMNT.mp4   -frames:v 1 -q:v 3 posters/LMNT.jpg
    ffmpeg -ss 35  -i Disney.mp4 -frames:v 1 -q:v 3 posters/Disney.jpg
    ffmpeg -ss 1   -i EatInADay.mp4 -frames:v 1 -q:v 3 posters/EatInADay.jpg
    ffmpeg -ss 0.5 -i Heat.mp4      -frames:v 1 -q:v 3 posters/Heat.jpg
    ffmpeg -ss 0.4 -i Shopping.mp4    -frames:v 1 -q:v 3 posters/Shopping.jpg
    ffmpeg -ss 38  -i DisneyTryOn.mp4 -frames:v 1 -q:v 3 posters/DisneyTryOn.jpg

## Replacing a video

Drop the new file in as `AB.mp4`, `LMNT.mp4` or `Disney.mp4` (H.264 + AAC,
faststart), grab a new poster frame with the command above, and update the
duration in `index.html` if you want the printed value correct before the
first play.

## Photos

`photos/maya-portrait.jpg` is `Maya12097 (1).jpg` from the repo root, resized
from 4338x5422 (12MB) to 900x1125 for the web. The ratio is unchanged at 4:5,
which is also the aspect of the frame it sits in, so the full photo is shown
with nothing cropped:

    ffmpeg -i "../Maya12097 (1).jpg" -vf scale=900:1125:flags=lanczos -q:v 3 photos/maya-portrait.jpg
