# Create File List

echo file file1.mp4 >  mylist.txt 
echo file file2.mp4 >> mylist.txt
echo file file3.mp4 >> mylist.txt

# Concatenate Files

ffmpeg -f concat -i mylist.txt -c:a copy output.mp4

# Add sub.ass subtitles to input.mp4 video
ffmpeg -i input.mp4 -vf "ass=sub.ass" result.mp4

### **note**: convert srt to ass first via: 

ffmpeg -i subtitles.srt subtitles.ass

# Crop 20 pixels from the top, and 20 from the bottom:

ffmpeg -i in.mp4 -filter:v "crop=in_w:in_h-40" -c:a copy out.mp4

# Crop 40 pixels only from the top

ffmpeg -i in.mp4 -filter:v "crop=in_w:in_h-40:0:out_h" -c:a copy out.mp4

# Crop 40 pixels only from the bottom

ffmpeg -i input.mp4 -filter_complex "[0:v]crop=in_w:in_h-40:0:0[cropped]" -map "[cropped]" output.mp4

# Cut 8 seconds of the video starting from 3rd second and save it as a file 

ffmpeg -i movie.mp4 -ss 00:00:03 -t 00:00:08 -async 1 cut.mp4

# Extract Audio

ffmpeg -i video.mp4 -f mp3 -vn music.mp3

# Extract subs

ffmpeg -i input_file out.srt

# Strip audio stream away from video

ffmpeg -i INPUT.mp4 -codec copy -an OUTPUT.mp4

# Combine the two streams together (new audio with originally exisiting video)

ffmpeg -i INPUT.mp4 -i AUDIO.wav -shortest -c:v copy -c:a aac -b:a 256k OUTPUT.mp4

# Reverse video only:

ffmpeg -i input.mp4 -vf reverse reversed.mp4

# Create mosaic video (two vertical):

ffmpeg –i left.mp4 –i right.mp4 –filter_complex hstack=inputs=2 mosaic.mp4

# Reverse audio and video:

ffmpeg -i /storage/emulated/0/ffvid/frameCount.mp4 -vf reverse -af areverse reversed.mp4

# Create video as a static picture + mp3

ffmpeg -loop 1 -i image.png -i music.mp3 -vf "scale='min(1280,iw)':-2,format=yuv420p" -c:v libx264 -preset medium -profile:v main -c:a aac -shortest -movflags +faststart output.mp4

# Create a silent mp3 for 1 second

ffmpeg -f lavfi -i anullsrc=r=44100:cl=mono -t 1 -q:a 9 -acodec libmp3lame out.mp3

# Downloading coub for full length of audio

### https://www.npmjs.com/package/coub-dl

coub-dl -i https://coub.com/view/135nqc -o out.mp4 --loop 999

# To list the available formats type and download the chosen one:

youtube-dl -F URL
youtube-dl -f <number> URL

# To list all subs for a video:

youtube-dl --list-subs URL

# To download subs

youtube-dl --write-sub --sub-lang=en --convert-subs=srt --skip-download URL 

# Download youtube playlist with en subs

youtube-dl --write-sub --sub-lang=en --convert-subs=ass --skip-download --output "%(title)s.%(ext)s" -i PLpCqYR5zVhaq7k4KJuRXMcjD7PhFHenDL

for i in *.vtt; do ffmpeg -i "$i" "${i%.*}.ass"; done

rm \*.vtt

youtube-dl -f 18 --output "%(title)s.%(ext)s" -i PLpCqYR5zVhar6AngZd1RIG2_5sTMZsHMX

for i in *.mp4; do ffmpeg -y -i "$i" -vf "ass=${i%%.*}.en.ass" "${i%.*}.en.sub.mp4"; done 