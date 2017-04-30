# melody-graph
Create a video using melody graph of audio.

<a href="http://www.youtube.com/watch?feature=player_embedded&v=56Ja6zFgL-Y&t=2857
" target="_blank"><img src="http://img.youtube.com/vi/56Ja6zFgL-Y/0.jpg" 
alt="Example" width="480" height="360" border="0" /></a>

<a href="http://www.youtube.com/watch?feature=player_embedded&v=56Ja6zFgL-Y&t=2857
" target="_blank"> Sample Video [Youtube] </a>

# Workflow with single sound
    music-grapher audio-file.m4a
    
    # Helpful settings for instruments
    # --mel_minpeaksalience=1.0 
    # --mel_voicing=5.00  
    # --freq_min=50 
    # --freq_max=700

# Workflow With Split sound
    ## Create temp directory
    mkdir -p raw
    
    ## Split sound 29 seconds each
    ffmpeg -i audio.m4a -f segment -segment_time 29 -c copy raw/sound-%3d.m4a
    
    ## produce videos
    
    ls raw/*.m4a | parallel music-grapher {}
    
    # see music-grapher -h for all args such as tonic adjustments
    
    ## merge with sound
    ## Remove old files if exists
    # rm raw/*-merged.mp4
    ls raw/sound-*.m4a | parallel ffmpeg -i {}.mp4 -i {}  -c:v copy -c:a copy {}-merged.mp4
    
    ## merge all
    ## Remove old if exists
    rm parts.txt    
    
    for f in ./raw/*-merged.mp4; do echo "file '$f'" >> parts.txt; done
    ffmpeg -f concat -safe 0 -i parts.txt -c copy final.mp4
