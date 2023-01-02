## What does this do?
This Python script is used to analyze embeddings trained with textual inversion using the [Automatic1111 Web UI](https://github.com/AUTOMATIC1111/stable-diffusion-webui). When ran, it will create graphs of the loss rate of the embedding and the magnitude of its vectors. It's safe to run while the embedding is being trained.

## Screenshots
Example of the script being run

![cmd](https://i.imgur.com/8SCCnsX.jpg)

It will output two images:

Image #1: TestEmbed-[step]-loss.jpg, which plots the loss rate from the textual_inversion_loss.csv file. In your A1111 settings, set the "Save an csv containing the loss to log directory every N steps, 0 to disable" setting to 1 for best results. Ideally you want a loss rate average to be less than 0.30.

![TestEmbed-500-loss](https://i.imgur.com/i2BvuM0.jpg)

Image #2: TestEmbed-[step]-vector.jpg, which plots the magnitude of each vector inside the embedding. The higher the magnitudes, the less flexible the the embedding will be to other words in a prompt.

In this example, the embedding had a learning rate of: 0.05:10, 0.02:20, 0.01:60, 0.005:200, 0.002:500, 0.001:3000, 0.0005

![TestEmbed-500-vector](https://i.imgur.com/A5AbHpQ.jpg)

Since each token in an embedding has 768 vectors, this can add up to a lot of lines being plotted and end up in a jumbled mess. In the .py file you set `VECTOR_GRAPH_CREATE_LIMITED_GRAPH = True` and change the `VECTOR_GRAPH_LIMITED_GRAPH_NUM_VECTORS` variable to limit how many are plotted.

![TestEmbed-500-vector-(100-vector-limit)](https://i.imgur.com/F3ZWiHD.jpg)

If all you want to do is inspect an existing embedding file that you downloaded from the internet, you can use `--file EmbeddingName` with the EmbeddingName.pt file next to the script.

![file](https://i.imgur.com/S4SHU0z.jpg)

## How to use
* Download the script by clicking the green "Code" button up top and then Download ZIP.
* Unzip it
* Place the inspect_embedding_training.py file at "Stable Diffusion\textual_inversion\YYYY-MM-DD\YourEmbeddingName" in your Automatic1111 installation.
* If you have Python set to run .py files, just double click the file to run it. Otherwise, open a command window in the directory and type `python inspect_embedding_training.py` and hit Enter.

![folder](https://i.imgur.com/tiM89rS.jpg)

If you get the error `ModuleNotFoundError: No module named 'torch'` when running the script, open a console and run the command `pip install torch` and try again.

## Configuration
Open the inspect_embedding_training.py file with notepad to edit some variables near the top of the file:
```python
SAVE_LOSS_GRAPH_IMG: bool = True
SAVE_VECTOR_GRAPH_IMG: bool = True

SHOW_PLOTS_AFTER_GENERATION: bool = False

GRAPH_IMAGE_SIZE: tuple[int, int] = (19, 9)
GRAPH_SHOW_TITLE: bool = True

VECTOR_GRAPH_CREATE_FULL_GRAPH: bool = True
VECTOR_GRAPH_CREATE_LIMITED_GRAPH: bool = False
VECTOR_GRAPH_LIMITED_GRAPH_NUM_VECTORS: int = 100
VECTOR_GRAPH_SHOW_LEARNING_RATE: bool = True
```

## Optional launch arguments
* `--help, -h` Shows help text.
* `--dir` The "/path/to/embedding/folder" to use instead of the local path where this script is at. This directory should have the textual_inversion_loss.csv file in it.
* `--out` The "/path/to/an/output/folder" to use instead of the local path for outputting images.
* `--file` The "EmbeddingName.pt" to inspect. Prints the embedding's: internal name, model name/hash, number of vectors per token, training step count, and average vector strength/magnitude

## Changelog
#### 1/01/2023
* Added `--file` launch arg to inspect an individual embedding file to get its internal info: internal name, model name/hash, number of vectors per token, training step count, and average vector strength/magnitude
* Now displays the average vector's strength/magnitude in the console and right hand side of the graph
#### 12/28/2022
* Initial release

## Special thanks
Shondoit - for supplying the base code for loading and graphing the data.
