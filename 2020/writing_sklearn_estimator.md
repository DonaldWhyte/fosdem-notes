# How to write a scikit-learn compatible estimator/transformer

https://fosdem.org/2020/schedule/event/python2020_scikit_learn_estimator/

* Estimators -- they are transformers or predictors
* Transformers -- take some data, spit it out in a different format (e.g. text to tf/idf transformer)
    * fit(array) --> updates internal learnt parameters based on input data
    * transformer(array) --> transformed data (for live)
* Predictors -- Take some data and output prediction (e.g. linear rgression)
    * fit(array) --> updates internal learnt parameters based on input data
    * predict(array) --> output predictions (for live)
* Scorers -- score the quality/correctness of output predictions (e.g. cross val)

We use a number of _hyperparameters_ for estimators (i.e. transformers and predictors).
    hyperparameters are set in the `__init__` of estimators

Use GridSearch to find best hyper parameters
    pass it your estimator pipeline
    your scorer
    and your space of parameters to search

Lots of estimators in sklearn.

But what if you need to write a custom estimator?
    e.g. if you're writing a data transformer, very common it won't be in library

Somehwat tricky to do since API is quite complex. Best way is to get started with the simple example here:

https://scikit-learn.org/dev/developers/develop.html

There's an awesome sklearn test decorate that triggers a test failure if you're classifier doesn't conform to the proper API.

A new experimental feature has been added to sklearn. This allows you to "tag" a custom classifier to stat that it only allows data of a certain type, what types of classifications/predictions it can handle (e.g. single label, etc.). These tags are read by grid search, etc. So you can write simplified classifiers that don't support all cases and still use it in sklearn.

Can also go o :

https://scikit-learn.org/dev/developers/develop.html
