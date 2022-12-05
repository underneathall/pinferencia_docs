# Machine Learning Frameworks

Here is how to load models from different framework:

=== "Scikit-Learn"

    ```python title="app.py"
    import joblib

    from pinferencia import Server


    # train your model
    model = "..."

    # or load your model
    model = joblib.load("/path/to/model.joblib") # (1)

    service = Server()
    service.register(
        model_name="mymodel",
        model=model,
        entrypoint="predict", # (2)
    )
    ```

    1. For more details, please visit https://scikit-learn.org/stable/modules/model_persistence.html

    2. `entrypoint` is the function name of the `model` to perform predictions.

        Here the data will be sent to the `predict` function: `model.predict(data)`.

=== "PyTorch"

    ```python title="app.py"
    import torch

    from pinferencia import Server


    # train your models
    model = "..."

    # or load your models (1)
    # from state_dict
    model = TheModelClass(*args, **kwargs)
    model.load_state_dict(torch.load(PATH))

    # entire model
    model = torch.load(PATH)

    # torchscript
    model = torch.jit.load('model_scripted.pt')

    model.eval()

    service = Server()
    service.register(
        model_name="mymodel",
        model=model,
    )
    ```

    1. For more details, please visit https://pytorch.org/tutorials/beginner/saving_loading_models.html

=== "Tensorflow"

    ```python title="app.py"
    import tensorflow as tf

    from pinferencia import Server


    # train your models
    model = "..."

    # or load your models (1)
    # saved_model
    model = tf.keras.models.load_model('saved_model/model')

    # HDF5
    model = tf.keras.models.load_model('model.h5')

    # from weights
    model = create_model()
    model.load_weights('./checkpoints/my_checkpoint')
    loss, acc = model.evaluate(test_images, test_labels, verbose=2)

    service = Server()
    service.register(
        model_name="mymodel",
        model=model,
        entrypoint="predict",
    )
    ```

    1. For more details, please visit https://www.tensorflow.org/tutorials/keras/save_and_load

=== "Any Model"

    ```python title="app.py"
    from pinferencia import Server


    class MyModel:
        def predict(self, data):
            return sum(data)


    model = MyModel()

    service = Server()
    service.register(
        model_name="mymodel",
        model=model,
        entrypoint="predict",
    )
    ```

=== "Any Function"

    ```python title="app.py"
    from pinferencia import Server

    def model(data):
        return sum(data)

    service = Server()
    service.register(
        model_name="mymodel",
        model=model,
    )
    ```
