Two examples of simulations of mixed effects models
==================================================

These are slides presented by [Dave Kleinschmidt](http://www.bcs.rochester.edu/people/dkleinschmidt/) at the [SST2014](http://www.nzilbb.canterbury.ac.nz/SST.shtml) [workshop on mixed effects models](http://www.nzilbb.canterbury.ac.nz/SST_TD_Jaeger.html).

They demonstrate how to use simulations to understand the behavior of different types of models.  Specifically, they generate fake data following the assumptions of a linear mixed model, and then fit a range of different models (including the generating model).  By generating the data yourself, you know whether there is really and effect or not, and by repeating the simulations many times you can estimate whether or not one kind of model (e.g. a mixed effects model with only random intercepts) makes statistically correct inferences for the type of data you generated (e.g. fewer than 5% false positives, or effect detected when no effect was actually present).

There are two examples:

False positives from ignoring random slopes
-------------------------------------------

When there _is_ variability in the strength of an effect across subjects, ignoring this in your model by not including random _slopes_ for that subject can lead to vastly inflated false positive (Type I) error rates.  To investigate this, we generate many random data sets assuming that there is _no_ true effect on average across subjects, but that there is variability (so that some subjects will show positive, and some negative, and some very near zero).  The correct inference in this case is that there's not sufficient evidence to reject the null hypothesis that there's no effect on average (fixed effect coefficient is zero).

When the presence of random slopes in the data generating process are ignored (either by fitting a fixed-effects only model with `lm` or by fitting random intercepts only) then the random variability across subjects is overfit, because each individual _observation_ is treated as a single independent observation about the underlying effect, reducing the standard error of the slope estimate and inflating the rate of false positives.

Tradeoff between power and false positive rate
----------------------------------------------

What about when there _is_ a true effect?  Our ability to detect it depends on the amount of data we have.  In a mixed effects model, this is constrained by the number of _subjects_ we have data for, in addition to how many observations we get from each subject.  To see this, we can generate data sets like above, but with an actual effect.  The size of the effect---the relative size of the true fixed effect coefficient, the random slope variance, and the residual variance---is set based on the frequency effect in the `lexdec` dataset in the `languageR` package, using a discretized high vs. low frequency contrast.  Random data sets are generated using these parameters with 6, 12, or 24 simulated subjects, and the same three models are fit as in the first example.  Additionally, datasets with no frequency effect are also generated to evaluate the false positive rate.

The most obvious conclusion is that power goes up with more subjects: when there's an effect, the t values of all three methods are larger (on average) for larger numbers of subjects.  Of all three methods, the random slopes model has the lowest power (as low as 65-70% for 6 subjects).  This might seem like an argument for using the less conservative random intercept or fixed effect models, but these models fail to control the false-positive rate, so the increased power comes at the price of more false positives.