Json Schema Model
=================

.. image:: https://travis-ci.org/philpep/jsonschema-model.svg?branch=master
   :target: https://travis-ci.org/philpep/jsonschema-model
   :alt: Build status

.. image:: https://img.shields.io/pypi/dm/jsonschema-model.svg
   :target: https://pypi.python.org/pypi/jsonschema-model/
   :alt: Downloads

.. image:: https://img.shields.io/pypi/v/jsonschema-model.svg
   :target: https://pypi.python.org/pypi/jsonschema-model/
   :alt: Latest Version

.. image:: https://img.shields.io/pypi/l/jsonschema-model.svg
   :target: https://pypi.python.org/pypi/jsonschema-model/
   :alt: License


Build python objects using JSON schemas::

    >>> import jsonschema_model
    >>> Model = jsonschema_model.model_factory({
    ...    "name": "Model",
    ...    "properties": {
    ...        "foo": {"type": "string"},
    ...        "bar": {"type": "array", "items": {
    ...            "type": "object",
    ...            "name": "Bar",
    ...            "properties": {
    ...                "zaz": {"type": "string"},
    ...            },
    ...        }},
    ...    }})

    # Simple object creation
    >>> obj = Model(foo="bar")
    >>> assert obj == {"foo": "bar"}

    # Nested and array are implemented
    # HINT: Use add() instead of append()
    >>> obj.bar.add(zaz="qux")
    {'zaz': 'qux'}
    >>> assert obj == {'foo': 'bar', 'bar': [{'zaz': 'qux'}]}

    # You can access via attribute or via dict like interface
    >>> obj["bar"][0].zaz
    'qux'

    # Array have a special get_or_create() method
    # to avoid dupplicates within an array
    >>> obj.bar.get_or_create(zaz="xuq")
    {'zaz': 'xuq'}
    >>> obj.bar
    [{'zaz': 'qux'}, {'zaz': 'xuq'}]
    >>> obj.bar.get_or_create(zaz="xuq")
    {'zaz': 'xuq'}
    >>> obj.bar
    [{'zaz': 'qux'}, {'zaz': 'xuq'}]
