# Audio-Summarizer-Transformers

This Project creates a web-based application using Gradio that transcribes and summarizes audio files. Here's a breakdown of the key components:

  Library Imports:
  transformers, torchaudio, gradio, and accelerate are imported for building the speech-to-text, summarization, and web interface parts of the application.
  torch is used for handling tensors and leveraging GPU acceleration if available.

  Model Loading:
  The code checks for GPU availability and sets the device accordingly (cuda or cpu).
  Transcription Model: It loads the whisper-small model from OpenAI using WhisperProcessor and WhisperForConditionalGeneration. This model is used to convert spoken audio into text.
  Summarization Model: It loads a BART-based summarization model (philschmid/bart-large-cnn-samsum) using AutoTokenizer and AutoModelForSeq2SeqLM. This model is fine-tuned for summarizing conversational text.
  
  Utility Functions:
  transcribe_audio_chunk(waveform, sample_rate): This function takes an audio waveform and its sample rate, resamples it to 16kHz (required by the Whisper model), processes it, and returns the transcribed text.
  
  summarize_text(text): This function takes text as input, tokenizes it, generates a summary using the loaded summarization model, and returns the summarized text.

  split_audio(file_path, chunk_duration=30): This function loads an audio file, splits it into smaller chunks of a specified duration (default is 30 seconds), and returns a list of these   chunks along with their sample rate. This is useful for processing long audio files that might exceed the model's input limits.

Gradio Interface Logic (process_audio function):
  This function is the core logic for the Gradio interface.
  It takes the audio file path as input.
  It initializes empty lists to store transcripts and summaries for each chunk.
  It calls split_audio to divide the input audio into chunks.
  It iterates through each audio chunk, transcribes it using transcribe_audio_chunk, and summarizes the transcript using summarize_text.
  It joins all the chunk transcripts to create the full_conversation.
  It generates a final_summary of the entire conversation using summarize_text.
  It formats the individual chunk transcripts and summaries into a readable string (chunk_outputs).
  Finally, it returns the formatted chunk outputs and the final summary.
  
  Gradio UI:
  inputs: Defines the input component for the Gradio interface, which is an audio input that accepts file uploads or recordings.
  outputs: Defines the output components, which are a Markdown box to display chunk-wise transcripts and summaries, and a Textbox to display the final full audio summary.
  gr.Interface: Creates the Gradio web interface, linking the process_audio function to the defined inputs and outputs.
  app.launch(): Launches the Gradio application. The share=True option is automatically set in Colab to generate a public URL for the application.

  Whisper (openai/whisper-small): This is an automatic speech recognition (ASR) model developed by OpenAI. It's trained on a large dataset of diverse audio and is capable of transcribing speech into text in multiple languages. The "small" version indicates the size of the model, which affects its performance and resource requirements. In this code, it's used to convert the audio input into written transcripts.
  
  BART (philschmid/bart-large-cnn-samsum): BART is a transformer-based model developed by Facebook. It's a sequence-to-sequence model that's particularly effective for tasks like summarization. The philschmid/bart-large-cnn-samsum version is a fine-tuned version of BART specifically trained on the SAMSum dataset, which consists of messenger-like conversations. This makes it well-suited for summarizing conversational text, as is done with the transcribed audio in this application. The "large" in the name refers to the model's size, and "cnn" often indicates that it was pre-trained on a large corpus like Common Crawl.

In essence, this code provides a complete workflow for taking an audio file, breaking it down, transcribing each part, summarizing each part, and then summarizing the entire transcription, presenting the results in a user-friendly web interface.
