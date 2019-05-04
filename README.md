The basic idea of this algorithm is fairly straight forward:

Do neural network search for depth and breadth of a network, as well as other hyper parameters.

How the algorithm works:

We do a binary tree traversal over two lists:
* the depth and breadth of the network
* the regret path or loss at the first 10 iterations

Once we have our first 10 iterations done, we then chose the best performing networks (maybe the top 10 or 20), then we run the search again, setting the boundary between those ten networks.  We train for 20 iterations this time.  Those with the best loss or best regret path are then chosen.

Then we search again on the remaining set and so on and so forth.

We then randomly initialize a second set of neural networks and carry out the same routinue getting to the best network architecture.

Then we repeat this again with new random initialization until we have some number of candidates.

Then we do a final round of the above binary search training for 100 or 200 iterations across the networks.  We choose the one with the best loss or regret path.  For vanilla neural networks this is enough.

As an optimization, we could take the final round or some intermediate round and being to iterate over the space of activation functions and the space of initializers.

Finally we could iterate over the space of loss functions.

Training should always happen with respect to a test set and validation should occur at the end of each round.

We could include some of the training set in the test set, but never in the validation.  The idea being, does the network learn what it's taught?  I think this is actually a valuable question.  It should never be a large percentage of what you test against, but validating against some information you trained against in light of new information might be an interesting breakdown.  It should be no more than like 5% of the examples to test against and never in the validation set.  And maybe it should be a mix of outliers and typical cases from the training set.

As a final optimization, we could randomly order the data, to make sure that early examples aren't being over biased too much.

Finally gradient clipping, batching and all the other optimizations and regularizations should be applied to the network.  And then we call the network trained.

