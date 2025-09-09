# AutoCoder ESP32

AutoCoder ESP32 is a web-based tool that uses AI to automatically generate, debug, and upload Arduino code to ESP32 boards. Simply provide a prompt for your project, and the tool will handle the rest, including compiling the code with the Arduino CLI and verifying its functionality via the serial port.

## Features

-   **AI-Powered Code Generation**: Utilizes Google's Gemini AI to generate Arduino code for the ESP32 based on user prompts.
-   **Automatic Code Rectification**: If the initial code fails to compile or upload, the AI will attempt to fix the errors and retry.
-   **Serial Monitoring**: Includes a serial monitor to confirm that the ESP32 has started up correctly after the code has been uploaded.
-   **User-Friendly Web Interface**: A simple web frontend allows for easy interaction and configuration.
-   **Safe Configuration**: API keys and other sensitive information are managed safely through a local configuration file that is not tracked by version control.

## Project Structure

The project is organized into a simple frontend and backend structure:

```
.
├── backend_app.py        # The main Flask backend application
├── config.py             # Local configuration for API keys (ignored by Git)
├── config_template.py    # A template for the config.py file
├── frontend/
│   └── index.html        # The single-page React frontend
├── requirements.txt      # A list of the Python dependencies
└── README.md             # This file
```

## Prerequisites

Before you begin, ensure you have the following installed:

-   **Python 3.7+**: The backend is built with Flask.
-   **Arduino CLI**: Required for compiling and uploading the ESP32 code. You can find the installation instructions [here](https://arduino.github.io/arduino-cli/latest/installation/).
-   **ESP32 Board Support for Arduino CLI**: You'll need to install the ESP32 board support for the Arduino CLI. You can do this by running the following command:
    ```bash
    arduino-cli core install esp32:esp32
    ```
-   **An ESP32 Board**: The hardware that the code will be uploaded to.
-   **A Google Gemini API Key**: You can get one from [Google AI Studio](https://aistudio.google.com/app/apikey).

## Setup and Installation

1.  **Clone the Repository**:
    ```bash
    git clone <repository-url>
    cd <repository-directory>
    ```

2.  **Set Up the Backend**:
    -   Install the required Python packages:
        ```bash
        pip install -r requirements.txt
        ```
    -   Create a `config.py` file from the template:
        ```bash
        cp config_template.py config.py
        ```
    -   Open `config.py` and add your Google Gemini API key:
        ```python
        # config.py
        GEMINI_API_KEY = "YOUR_GEMINI_API_KEY"
        ```

3.  **Configure the Arduino CLI**:
    -   Make sure that the `arduino-cli` command is in your system's PATH.
    -   Connect your ESP32 board to your computer and find its serial port name (e.g., `/dev/ttyUSB0` on Linux, `COM3` on Windows).

## Usage

1.  **Start the Backend Server**:
    ```bash
    python backend_app.py
    ```
    The server will start on `http://127.0.0.1:5000`.

2.  **Open the Frontend**:
    -   Open the `frontend/index.html` file in your web browser.

3.  **Generate and Upload Code**:
    -   In the web interface, enter a prompt describing what you want the ESP32 to do (e.g., "Blink an LED on pin 2").
    -   (Optional) Provide any specific pin configurations.
    -   (Optional) Enter your Wi-Fi credentials if your project requires an internet connection.
    -   Make sure the ESP32 serial port is correct.
    -   Click the "Generate Code & Upload to ESP32" button.

The application will then generate the code, compile it, and upload it to your ESP32. You can monitor the progress in the output log on the web page.

## How It Works

The workflow of the application is as follows:

1.  The user enters a prompt into the web frontend and clicks the "Generate" button.
2.  The frontend sends the prompt and other configuration details to the Flask backend.
3.  The backend sends a request to the Google Gemini API to generate the Arduino code.
4.  The generated code is saved to a temporary file.
5.  The `arduino-cli` is used to compile the code.
6.  If the compilation is successful, the `arduino-cli` is used to upload the code to the ESP32.
7.  After the upload, the backend monitors the serial port for a "ESP32 Ready!" message to confirm that the code is running.
8.  If any step fails, the AI will attempt to rectify the code, and the process will be repeated up to a maximum number of retries.

## Contributing

Contributions are welcome! If you have any ideas, suggestions, or bug reports, please open an issue or submit a pull request.

## License

This project is licensed under the MIT License. See the `LICENSE` file for more details.
