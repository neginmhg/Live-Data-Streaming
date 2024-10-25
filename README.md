# Live Data Streaming App

This project is a **Live Data Streaming App** that fetches video data from a specified YouTube playlist and streams the data to a Kafka topic using Avro serialization. The app integrates with the YouTube Data API to retrieve video details like title, views, likes, and comments, and then sends the summarized data to a Kafka topic for real-time processing.

## Prerequisites

To run this project, you'll need the following:

1. **Python 3.x** - Make sure Python is installed.
2. **Kafka Broker** - Set up and run a Kafka broker with a Schema Registry.
3. **YouTube Data API Key** - Get an API key from the [Google Developer Console](https://console.developers.google.com/).
4. **Confluent Kafka Libraries** - You'll need the Confluent Kafka Python client for Kafka and Schema Registry interaction.

## Installation

1. **Clone the repository** and navigate to the project directory:

    ```bash
    git clone https://github.com/your-username/live-data-streaming-app.git
    cd live-data-streaming-app
    ```

2. **Install the required dependencies**:

    ```bash
    pip install requests confluent-kafka pprint
    ```

3. **Create a `config.py` file** in the root directory and add your configuration details. Here's an example:

    ```python
    config = {
        "google_api_key": "YOUR_GOOGLE_API_KEY",
        "youtube_playlist_id": "YOUR_YOUTUBE_PLAYLIST_ID",
        "schema_registry": {
            "url": "http://localhost:8081"
        },
        "kafka": {
            "bootstrap.servers": "localhost:9092",
            "schema.registry.url": "http://localhost:8081",
            "topic": "youtube_videos"
        }
    }
    ```

4. **Ensure that Kafka and Schema Registry are running** on your local environment or a server.

## Usage

Once the setup is done, run the script:

```bash
python your_script_name.py
```

This will start fetching video details from the specified YouTube playlist and produce them to the Kafka topic.

### Key Features

- **YouTube API Integration**: Fetches video details such as title, views, likes, and comments from a YouTube playlist.
- **Kafka Producer**: Sends the fetched video details to a Kafka topic for real-time data streaming.
- **Avro Serialization**: Uses Avro serialization for the Kafka messages with schemas managed via Confluent's Schema Registry.

## Configuration Details

- **YouTube API Key**: This is used for authenticating requests to the YouTube Data API. The `google_api_key` must be added in the `config.py` file.
- **Kafka Configuration**: The `config.py` file also contains Kafka and Schema Registry connection details. Make sure the `bootstrap.servers` and `schema.registry.url` are set correctly for your Kafka setup.
- **Schema Registry**: Avro serialization is utilized to serialize video data. The latest schema for the topic is fetched from the Schema Registry.

## How the App Works

1. **Fetching Playlist Items**: 
   - The app fetches videos from a YouTube playlist using the YouTube Data API through the `fetch_playlist_items()` function.
   
2. **Fetching Video Details**: 
   - For each video, the app retrieves additional details (title, views, likes, comments) via the `fetch_videos()` function.

3. **Summarizing Video**: 
   - The `summarize_video()` function creates a simplified dictionary containing the video summary.

4. **Producing to Kafka**: 
   - The summarized video details are sent to the Kafka topic defined in the `config.py` file. Data is serialized using Avro, with schema management provided by the Schema Registry.

## Logging

The app logs important information such as video data as it processes them. The logging level is set to `INFO` by default, but you can change this in the script if needed:

```python
logging.basicConfig(level=logging.INFO)
```

## Error Handling

The `on_delivery()` callback is used to monitor whether each Kafka message was successfully delivered. You can customize this function to handle failures or retries based on your needs.

## Project Structure

```bash
live-data-streaming-app/
│
├── config.py                 # Configuration file (API key, Kafka, Schema Registry settings)
├── your_script_name.py        # Main script for data streaming
├── README.md                  # Project documentation
├── requirements.txt           # Dependencies
└── LICENSE                    # Project license
```
