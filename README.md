# Torch Model Manager


**Torch Model Manager** is an open-source python project designed for Deep Learning developpers that aims to make the use of pytorch library easy. The version ![version](https://img.shields.io/badge/version-0.0.5-gray?labelColor=blue&style=flat) is still under developpment. The package allows us to access, search and delete layers by index, attributes or instance.

### Examples of Use
1. **Initialization**
```python
from torchvision import
from torch_model_manager import TorchModelManager

# Assume you have a PyTorch model 'model'
model = models.vgg16(pretrained=True)

model_manager = TorchModelManager(model)
```

2. **Get Named Layers**
```python
named_layers = model_manager.get_named_layers()

# output
>>> {
    'features': {
        '0': 'Conv2d(3, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))',
        '1': 'ReLU(inplace=True)',
        '2': 'Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))',
        '3': 'ReLU(inplace=True)',
        '4': 'MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)',
        '5': 'Conv2d(64, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))',
        '6': 'ReLU(inplace=True)',
        '7': 'Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))',
        '8': 'ReLU(inplace=True)',
        '9': 'MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)',
        '10': 'Conv2d(128, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))',
        '11': 'ReLU(inplace=True)',
        '12': 'Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))',
        '13': 'ReLU(inplace=True)',
        '14': 'Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))',
        '15': 'ReLU(inplace=True)',
        '16': 'MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)',
        '17': 'Conv2d(256, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))',
        '18': 'ReLU(inplace=True)',
        '19': 'Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))',
        '20': 'ReLU(inplace=True)',
        '21': 'Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))',
        '22': 'ReLU(inplace=True)',
        '23': 'MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)',
        '24': 'Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))',
        '25': 'ReLU(inplace=True)',
        '26': 'Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))',
        '27': 'ReLU(inplace=True)',
        '28': 'Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))',
        '29': 'ReLU(inplace=True)',
        '30': 'MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)',
    },
    'avgpool': 'AdaptiveAvgPool2d(output_size=(7, 7))',
    'classifier': {
        '0': 'Linear(in_features=25088, out_features=4096, bias=True)',
        '1': 'ReLU(inplace=True)',
        '2': 'Dropout(p=0.5, inplace=False)',
        '3': 'Linear(in_features=4096, out_features=4096, bias=True)',
        '4': 'ReLU(inplace=True)',
        '5': 'Dropout(p=0.5, inplace=False)',
        '6': 'Linear(in_features=4096, out_features=1000, bias=True)'
    }
}

```

This method allows the user to access the overall architecture of the model in dictionnary format.


3. **Get Layer by Index**
```python
layer_index = ['classifier', 6]
layer = model_manager.get_layer_by_index(layer_index)

>>> Linear(in_features=4096, out_features=1000, bias=True)
```

The index is represented by a list, where each position represents a level. For instance, in the previous example, 'classifier' is the index to access the first level of the model architecture, and 6 is the index of the layer at the second level.

4. **Get Layer by Attribute**
```python
layers = model_manager.get_layer_by_attribute('out_features', 1000, '==')
>>> {('classifier', 6): Linear(in_features=4096, out_features=1000, bias=True)}
```
Retrieves all the layers satifying the given condition `out_features = 1000`.

5. **Get Layers by Conditions**
```python
# Retrieve layers that satisfy the given conditions
conditions = {
            'and': [{'==': ('kernel_size', (1, 1))}, {'==': ('stride', (1, 1))}],
            'or': [{'==': ('kernel_size', (3, 3))}]
            }
layers = model_manager.get_layer_by_attributes(conditions)
>>> {('features', 0): Conv2d(3, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 5): Conv2d(64, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 7): Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 10): Conv2d(128, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 12): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 14): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 17): Conv2d(256, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 19): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 21): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 24): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 26): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 28): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))}
```

It is almost the same as the previous one, but this time it extracts the layers that satisfy a set of conditions.

6. **Get Layer by Instance**
```python
# Search for layers in the model by their instance type
layers = model_manager.get_layer_by_instance(nn.Conv2d)
>>> {('features', 0): Conv2d(3, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 5): Conv2d(64, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 7): Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 10): Conv2d(128, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 12): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 14): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 17): Conv2d(256, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 19): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 21): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 24): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 26): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1)), ('features', 28): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))}
```

The `get_layer_by_instance` method allows you to extract layers of a specific type from the model. In the previous example, the extracted layers are convolutional layers.

7. **Delete Layer by Index**
The deletion process involves the following steps:

1. Search for the layers and retrieve their indexes.
2. Delete the layers at the corresponding indexes.

Here is an example of how to delete layers using different methods:

- Delete a layer by index:

```python
# Delete a layer from the model using its index
model_manager.delete_layer_by_index(['features', 0])
```

8. **Delete Layer by Attribute**
```python
# Delete layers from the model based on a specific attribute
model_manager.delete_layer_by_attribute('activation', 'relu', '==')
```
9. **Delete Layers by Conditions**
```python
# Delete layers from the model based on multiple conditions
conditions = {
    'and': [{'==': ('kernel_size', (1, 1))}, {'==': ('stride', (1, 1))}],
    'or': [{'==': ('kernel_size', (3, 3))}]
}
model_manager.delete_layer_by_attributes(conditions)
```
10. **Delete Layer by Instance**

```python
# Delete layers from the model by their instance type
model_manager.delete_layer_by_instance(nn.Conv2d)
```

# torch-model-manager-main
