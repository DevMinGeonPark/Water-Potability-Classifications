[한국어](README.md) | English

# Water Potability Classification

A machine learning study project that classifies whether water is safe to drink from its measured properties. It was built as coursework for the AI Systems Design class at Dongseo University in spring 2022, and the goal was to apply several classifiers to the same data and compare how they perform.

Authors: Mingeon Park and Chaeho Kim.

## Data

The project uses `data/water_potability.csv`. The inputs are nine water-quality measurements: ph, Hardness, Solids, Chloramines, Sulfate, Conductivity, Organic_carbon, Trihalomethanes, and Turbidity. The final column, `Potability` (1 = potable, 0 = not potable), is the target. There are roughly 3,276 rows in total.

The ph, Sulfate, and Trihalomethanes columns contain missing values. The notebook tries both filling them with the median and dropping the affected rows; the final run uses `dropna` to remove rows with missing values before analysis.

## Workflow

After preprocessing, the notebook proceeds as follows:

1. Inspect the data by plotting the `Potability` class balance, per-feature distributions (KDE), and a correlation heatmap with plotly and seaborn.
2. Split the data 70/30 into train/test sets and standardize it with `StandardScaler`.
3. Train several classifiers with arbitrary parameters for a first-pass comparison.
4. Search hyperparameters per model with `GridSearchCV`.
5. Run `RepeatedStratifiedKFold` cross-validation with the chosen parameters and compare mean accuracy.

## Models

The comparison covers classifiers from scikit-learn and Keras:

- LinearSVC and SVC (support vector machines)
- KNeighborsClassifier (KNN)
- MLPClassifier (scikit-learn multilayer perceptron)
- A deep neural network built in Keras (DMLP), wrapped with `KerasClassifier` so it fits the same pipeline.

Evaluation uses `precision_score` and `accuracy_score`.

## Notebooks

| File | Contents |
|---|---|
| `src/WPC.ipynb` | Main analysis, from preprocessing to model comparison |
| `src/WPC_plot.ipynb` | Same work with the rendered plots saved alongside |

## Running it

The main dependencies are numpy, pandas, scikit-learn, tensorflow/keras, matplotlib, seaborn, plotly, and missingno. After installing them, open `src/WPC.ipynb` in Jupyter and run the cells from the top.

```bash
pip install numpy pandas scikit-learn tensorflow matplotlib seaborn plotly missingno
jupyter notebook src/WPC.ipynb
```
