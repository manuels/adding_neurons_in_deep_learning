How to add new layers to deep neural networks?
==============================================

I am a newbie in deep learning and from my point of view the design of artificial neuronal networks is pretty much arbitrary. Sure, behind some design there is heuristic reasoning, but if you want to add another layer to improve the performance of your neuronal net, AFAIK there is no rule of thumb where to insert it into your neuronal network.

So I performed a little Gedankenexperiment this weekend with this small example:
Suppose you have a network with two input neurons and one output layer.

    (I)----
           \
            (O)
           /
    (I)----

The input neurons have an activation level in the interval [0,1] and the output neuron has a ReLU activation function

    f(a,w) = max(0, a*w + b) ∈ [0,1]

where `a` denotes the activation of the input layer (as a vector), `w` represents their corresponding weights and `b` the bias.

We know that according to the backpropagation algorithm the change of the cost function `C` with respect to the weights depends on the activation and the error `𝜹`

    ∂C/∂w = a*𝜹

Now lets us add a new (hidden) neuron into this network

    (I)---(H)--
               \
                (O)
               /
    (I)--------

Let us suppose that this hidden neuron has the same ReLU activation function with bias `c=0` and weight `v=1`. Since the activation is non-negative, with these parameters the ReLU activation function of this hidden neuron does not affect the operation of the network at all.

When we connect this hidden neuron with the other input layer.

    (I)---(H)--
         /     \
        /       (O)
       /       /
    (I)--------

If we assume that the weight of the newly introduced link is `0`, the weights of the hidden neuron become `v=[1, 0]`. Note that its weight `v` becomes a vector. However, the network with these these parameters is still identical to the network we started with.

Let us change the notation of the hidden neuron's weight vector to `v=[r*cos(θ​), r*sin(θ​)]` with `r=1`.
As long as `θ​=0`, the we still did not change anything from the network we started out with.
However, we can now see how the cost function changes when we change the weights of the hidden neuron so that it has an effect on the network

    ∂C/∂θ = (∂C/∂v) * (∂v/∂θ) = (a*𝜹) * [-r*sin(θ), r*cos(θ)]

Now we can see that if we introduce the new link into the network by changing `θ` away from `0`, the cost function changes proportional to `a*𝜹`.

[Just a side note: Since we are free to choose `r` and `θ`, if we change `θ`, we can choose `r` in such a way that `r*cos(θ)=1` still holds.]

*** So what does this tell us about introducing new neurons? ***

The new neurons have greatest impact on the cost function if we add the neurons next to those existing neurons where the activation `a` AND the error `𝜹` are the largest.

The argumentation here is just for a very small network but you can adopt this argumentation for larger networks where `(I)` are not input layers but hidden layers of a previous layer and `(O)` is a hidden layer of a latter layer:

    ...--(Hn-1)---(Hn)--
                 /     \
                /       (Hn+1)--...
               /       /
    ...--(Hn-1)--------

I have no idea if this argumentation is correct and if this has been shown before, but I would appreciate some [feedback on stackoverflow](https://news.ycombinator.com/item?id=11425188).

