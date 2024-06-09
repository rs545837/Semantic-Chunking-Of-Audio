# YouTube Video to Audio-Text Segmentation

This project provides a comprehensive pipeline to extract and align audio-text pairs from YouTube videos. It downloads a YouTube video, converts it to audio, performs voice activity detection (VAD), transcribes the audio, aligns the transcript, and combines the segments into audio-text pairs. The entire process is made accessible through a Gradio interface for ease of use.

## Table of Contents
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Detailed Explanation](#detailed-explanation)
- [Generalization](#generalization)
- [Potential Limitations](#potential-limitations)
- [Customization](#customization)

## Features
- Download YouTube videos.
- Convert video files to audio.
- Transcribe audio using the Whisper model.
- Align transcribed text with audio using the CTC Forced Aligner.
- Perform Voice Activity Detection (VAD) using the pyannote library.
- Combine VAD and text segments into audio-text pairs.
- User-friendly interface with Gradio.

  
## Detailed Explanation

### video2mp3 Function
Converts a video file to an audio file using FFmpeg. It takes the video file path and the desired output extension as input and returns the path of the generated audio file.

### download_video Function
Downloads a YouTube video using the pytube library. It selects the highest resolution progressive stream and downloads the video. It returns the path of the downloaded video file.

### transcribe Function
Transcribes the audio file using the Whisper model. It loads the "large-v3" model, transcribes the audio, and returns the transcribed text.

### align_transcript Function
Aligns the transcribed text with the audio using the ctc-forced-aligner library. It takes the audio file path and the transcript as input, writes the transcript to a temporary file, and runs the forced alignment using the specified parameters. It returns the path of the alignment output file.

### time_to_seconds Function
A helper function that converts a time string in the format "HH:MM:SS" or "MM:SS" or "SS" to seconds.

### parse_vad_file Function
Parses the Voice Activity Detection (VAD) output file and extracts the speech segments. It reads the file line by line, matches the start and end times using regular expressions, and returns a list of dictionaries containing the start and end times of each speech segment.

### parse_text_timestamps_file Function
Parses the aligned text timestamps file and extracts the text segments along with their start and end times. It reads the file line by line, matches the start time, end time, and text using regular expressions, and returns a list of dictionaries containing the start time, end time, and text of each segment.

### combine_segments Function
Combines the VAD segments with the text segments to create audio-text pairs. It iterates over each text segment, finds the overlapping VAD segments, and combines them into chunks of a specified maximum duration. It returns a list of dictionaries containing the chunk ID, chunk length, text, start time, and end time of each combined segment.

### perform_vad Function
Performs Voice Activity Detection on the audio file using the pyannote library. It loads a pre-trained segmentation model, applies it to the audio file, and saves the VAD results to a file.

### process_youtube_video Function
The main function that orchestrates the entire process. It takes a YouTube video URL as input, downloads the video, converts it to audio, performs VAD, transcribes the audio, aligns the transcript, parses the VAD and text segments, combines them, and returns the combined audio-text pairs as a JSON string.

## Generalization
This code can be applied to various types of videos, such as lectures, interviews, or presentations, where the goal is to align the spoken content with the corresponding text.

## Potential Limitations
- The accuracy of transcription and alignment may vary depending on audio quality, background noise, accents, and speaking styles.
- The code assumes that the video contains spoken content in English. For other languages, the models and libraries need to be adapted.
- The pre-trained VAD model may not be optimal for all audio types.

## Customization
To adapt the code for other languages or specific needs:
- Replace the Whisper model with a language-specific transcription model.
- Use a different alignment tool compatible with the target language.
- Adjust the regular expressions used for parsing the VAD and text timestamp files.
- Modify the maximum chunk duration based on the content nature.
