# Jarvis

So i was looking at the csv and thought `"kek tf does this mean"` but then i checked the range values for the csv and saw they were rly similar. i looked at the hint about tony stark and thought maybe i need to build an ai or some shit. woa kept telling me i was mental and had big confusion but i continued anyway. i built one \(code below\) and ran it \(followed tutorial with adjustments for massive training data\). the output it gave was in binary \(like in training data\). i converted to text and gave `flag{mlWis_cool}` but it looked like some bits were flipped so i changed it to `flag{ml_is_cool}` which is flag.

[https://medium.com/technology-invention-and-more/how-to-build-a-simple-neural-network-in-9-lines-of-python-code-cc8f23647ca1](https://medium.com/technology-invention-and-more/how-to-build-a-simple-neural-network-in-9-lines-of-python-code-cc8f23647ca1)

```python
hlp = open("help.csv","r").read().split("\n")
hlp = [[int(y) for y in x.split(",")] for x in hlp]

flg = open("flag.csv","r").read().split("\n")
flg = [[int(y) for y in x.split(",")] for x in flg]


from numpy import exp, array, random, dot, set_printoptions, inf
set_printoptions(threshold=inf)

class NeuralNetwork():
    def __init__(self):
        self.synaptic_weights = 2 * random.random((10, 1)) - 1
    def __sigmoid(self, x):
        return 1 / (1 + exp(-x))
    def __sigmoid_derivative(self, x):
        return x * (1 - x)
    def train(self, training_set_inputs, training_set_outputs, number_of_training_iterations):
        for iteration in range(number_of_training_iterations):
            output = self.think(training_set_inputs)
            error = training_set_outputs - output
            adjustment = dot(training_set_inputs.T, error * self.__sigmoid_derivative(output))
            self.synaptic_weights = self.synaptic_weights + adjustment # fukin numpy being shit kek
    def think(self, inputs):
        return self.__sigmoid(dot(inputs, self.synaptic_weights))



neural_network = NeuralNetwork()

print("Random starting synaptic weights: ")
print(neural_network.synaptic_weights)
training_set_inputs = [[y/100 for y in x[1:]] for x in hlp]
training_set_outputs = [x[0] for x in hlp]

print(training_set_inputs[0],training_set_outputs[0])

for i,j in enumerate(training_set_inputs):
    neural_network.train(array([j]), array(training_set_outputs[i]).T, 10000) # gotta train individually or numpy gets triggered

print("New synaptic weights after training: ")
print(neural_network.synaptic_weights)

b = ""
for i,j in enumerate(flg):
    print("Considering new situation :",j)
    a = neural_network.think(array(flg[i]))[0]
    b += str(int(a))
    print(int(a))
print(b)
```

