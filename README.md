# Lightfm

A Python implementation of LightFM, a hybrid recommendation algorithm.

The LightFM model incorporates both item and user metadata into the traditional matrix factorization algorithm. It represents each user and item as the sum of the latent representations of their features, thus allowing recommendations to generalise to new items (via item features) and to new users (via user features).

The details of the approach are described in the LightFM paper, available on [arXiv](http://arxiv.org/abs/1507.08439).

## Installation
Install from pypi using pip: `pip install lightfm`.

## Usage and examples
Model fitting is very straightforward.

Create a model instance with the desired latent dimensionality
```python
from lightfm import LightFM

model = LightFM(no_components=30)
```

Assuming `train` is a (no_users, no_items) sparse matrix (with 1s denoting positive, and -1s negative interactions), you can fit a traditional matrix factorization model by calling
```python
model = fit(train, epochs=20)
```
This will assume that each item and each user is described by their own specific feature.

To get predictions, call `model.predict`:
```python
predictions = model.predict(test.row, test_col)
```

User and item features can be incorporated into training by passing them into the `fit` method. Assuming `user_features` is a (no_users, no_user_features) sparse matrix (and similarly for `item_features`), you can call
```python
model = fit(train,
            user_features=user_features,
            item_features=item_features,
            epochs=20)
predictions = model.predict(test.row,
                            test_col,
                            user_features=user_features,
                            item_features=item_features)
```
to train the model and obtain predictions.

Both training and prediction can employ multiple cores for speed:
```python
model = fit(train, epochs=20, num_threads=4)
predictions = model.predict(test.row, test_col, num_threads=4)
```

Check the `examples` directory for more examples. The Movielens example shows how to use `lightfm` on the Movielens dataset, both with and without using movie metadata.

## Development
Pull requests are welcome. To install for development:

1. Clone the repository: `git clone git@github.com:lyst/lightfm.git`
2. Install it for development using pip: `cd lightfm && pip install -e .`
3. You can run tests by running `python setupy.py test`.

When making changes to the `.pyx` extension files, you'll need to run `python setup.py cythonize` in order to produce the extension `.c` files before running `pip install -e .`.