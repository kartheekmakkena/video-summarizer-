# Video Summarizer with Multimodal LLMs

This project demonstrates a powerful workflow for summarizing video content by leveraging computer vision and a local multimodal Large Language Model (LLM). The process involves extracting frames from a video, analyzing them in batches to generate descriptive summaries, and finally compiling a comprehensive overall summary of the entire video.

The example provided in the notebook processes an animated video of the classic fable, "The Hare and The Tortoise."

## How It Works

The summarization process is broken down into three main stages:

1.  **Frame Extraction**:
    *   The first script uses **OpenCV** to read a video file.
    *   It extracts frames at a specified interval (e.g., one frame every 0.5 seconds).
    *   These frames are saved as individual image files (`.png`) in a designated output directory.

2.  **Batch Analysis & Summarization**:
    *   The extracted frames are processed in batches (e.g., 10 frames at a time) to manage memory and context length.
    *   For each batch, the images are sent to a locally running multimodal LLM (`gemma3:27b` via **Ollama**).
    *   The LLM first generates detailed one-line descriptions for each image in the batch.
    *   It then creates a concise 3-line summary for that specific batch, capturing the key events within that short segment.

3.  **Final Aggregation**:
    *   The summaries from all batches are concatenated into a single text document.
    *   This combined text is then fed back into the LLM to generate a final, high-level summary that encapsulates the entire narrative of the video from start to finish.

This hierarchical approach allows the model to first focus on small details within short timeframes and then "zoom out" to understand the broader story arc.

## Features

-   **Video to Text**: Converts visual video information into a structured text summary.
-   **Local First**: Utilizes a locally hosted model with Ollama, ensuring privacy and no API costs.
-   **Batch Processing**: Efficiently handles longer videos by breaking them down into manageable chunks.
-.  **Hierarchical Summarization**: Creates detailed micro-summaries before generating a final comprehensive overview.
-   **Customizable**: Easily change the frame extraction rate, batch size, and LLM model.

## Prerequisites

Before you begin, ensure you have the following installed:

-   **Python 3.8+**
-   **Ollama**: Follow the installation guide at [https://ollama.com/](https://ollama.com/).
-   A multimodal model pulled in Ollama. This project uses `gemma3:27b`.
    ```sh
    ollama pull gemma3:27b
    ```
-   A video file you wish to summarize.

## Setup & Usage

1.  **Clone the Repository**
    ```sh
    git clone <your-repository-url>
    cd <repository-directory>
    ```

2.  **Install Dependencies**
    ```sh
    pip install -r requirements.txt
    ```
    *(If a `requirements.txt` file is not provided, install the necessary packages manually)*:
    ```sh
    pip install opencv-python ollama jupyter
    ```

3.  **Configure Paths in `video_summarizer.ipynb`**

    *   **Cell 1 (Frame Extraction):** Update the input video path and the output directory path for the frames.
      ```python
      # Example usage
      extract_frames_opencv(
          r"path/to/your/video.mp4",
          r"path/to/your/output_frames_folder",
          fps_interval=0.5 # 1 frame every 0.5 seconds
      )
      ```
    *   **Cell 2 (Batch Analysis):** Ensure the `folder_path` points to the directory where you saved the frames.
      ```python
      folder_path = r"path/to/your/output_frames_folder"
      ```

4.  **Run the Notebook**
    *   Start the Jupyter Notebook server:
      ```sh
      jupyter notebook
      ```
    *   Open `video_summarizer.ipynb`.
    *   Make sure your Ollama application is running in the background.
    *   Run the cells sequentially from top to bottom.

## Example Output

After running the entire notebook on the "The Hare and The Tortoise" video, the final output will be a comprehensive summary similar to this:

> ### Overall Summary of the Image Sequence (Images 1-80)
>
> This image sequence vividly illustrates the classic fable of "The Tortoise and the Hare." The story begins with a confident hare and a determined tortoise embarking on a race through a vibrant forest. The hare quickly takes a substantial lead, becoming overconfident and taking a nap while the tortoise steadily continues. As the hare sleeps, the tortoise perseveres, eventually overtaking him and winning the race. The sequence culminates in the tortoise triumphantly receiving a trophy, reinforcing the moral of the story: slow and steady wins the race. The images are bright, cartoonish, and effectively convey the narrative arc of this beloved tale.

---
