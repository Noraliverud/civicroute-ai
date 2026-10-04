# CivicRoute AI
Author: Nora Liverud 

CivicRoute AI is a Keras prototype of a possible solution for a city council to get help with categorising its public service requests. It is a prediction model trained to categorise incoming reports. Reports are split into four categories: potholes, broken streetlights, water leaks and illegal dumping. The model is intended to be used as a tool by city council employees.

## How to run it

To run this model you need Python 3.12. From the project folder, begin by installing the required packages:

```
pip install -r requirements.txt
```

Then open `civicroute_assessment.ipynb` in VS Code with Jupyter support and run all the cells from top to bottom.

## The data

The data used in this model comes from 600 made-up reports. 360 of the reports were used for training, 120 for validation and 120 for testing. 

The supplied dataset has 13 columns. From these, I selected 9 input fields: the hour it was reported, how many times it was reported, photo quality, age of the nearby asset, word count of the description, earlier fix time, channel (app, web, phone, office), urgency (low, medium, high) and recent rain (yes/no). After encoding, these become 15 numerical input columns. They are details about each report: the model does not read the description text or look at the photo itself.

The only dataset that was used came with the assignment. Locate the data dictionary in `reference/CivicRoute_AI_Data_Dictionary.pdf`.

Before training, I cleaned and prepared the data in the notebook. Missing numerical values were filled with the median from the training set, and missing `channel` values with the most common channel in the training set. The text categories (channel, urgency and recent rain) were turned into separate 0/1 columns so the model could read them. The supplied dataset is stored in `data/civic_requests_prepared.csv`; the preparation happens in memory in the notebook.

## The model

The model is a small neural network in Keras. It has one hidden layer with 16 units and an output layer with 4 units, one for each category. It trained for 30 epochs, with a batch size of 32 and a learning rate of 0.001. The model takes the 15 prepared numerical input columns and gives a probability for each of the 4 categories. The category with the highest probability becomes the prediction.


## Results

On the test set, the model got 75% right, which is 90 of the 120 reports. The final training accuracy was 78.3% and the final validation accuracy was 77.5%, so the two scores came out close. The model was best at recognising illegal dumping (27 of 30), followed by water leaks (25 of 30) and broken streetlights (24 of 30). It struggled most with potholes and only got 14 of 30 right. Nine of the 30 actual pothole reports were predicted as broken streetlights. The confusion matrix and training curves are in `outputs/confusion_matrix.png` and `outputs/training_curves.png`.

## Limitations

The model comes with a few limitations worth mentioning. 

- It struggles with categorising potholes
- The dataset is both small and made up
- Good results on made-up reports don't necessarily show how well it would work on real council reports
- It uses the supplied description word count, but does not read the actual description itself
- A council employee should always check its suggestion to confirm the result, before any action is taken

## Files

- `civicroute_assessment.ipynb`: the notebook I worked in during this assignment
- `data/`: the dataset used
- `outputs/`: charts, measurements and the saved model
- `reference/`: the document that explains the data fields
- `requirements.txt`: the Python packages the project needs
