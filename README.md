mongoexhaust
========

MongoExhaust lets you return MongoEngine documents and querysets as JSON in Flask, primarily for production/consumption as a JSON API endpoint. It provides JSON-compatible representations of BSON datatypes including `ObjectId`, `DBRef`, and `datetime`. MongoExhaust also passes all emissions standards (except for in California).

Brought to you by [wiota.co](http://wiota.co).

## Requirements

Flask and MongoEngine are the only non-built-in requirements:

    $ pip install flask
    $ pip install mongoengine

You'll probably also want `flask-mongoengine`:

    $ pip install flask-mongoengine

## Usage

In your `app.py`, or wherever:

    import mongoexhaust

MongoExhaust furnishes the following MongoEngine classes with the following output functions:

- `Document.to_dict()` - produces a `dict` representation of the Document
- `Document.to_bson()` - produces a Flask-Response a la `jsonify()`
- `QuerySet.to_dict()` - produces a `dict` representation of the QuerySet
- `QuerySet.to_bson()` - produces a Flask-Response a la `jsonify()`

So wherever you want to use it, use it, e.g., in an API endpoint:

    from my_special_models import SuperModel
    
    @app.route('/api/v1/model/super')
    def super_model(model_name):
        return SuperModel.objects.get(name=model_name).to_bson()

## Tips

- MongoExhaust provides a function to wrap the data passed to the output of `to_bson`, specifically to do things such as add a `{"result" : "..."}` wrapper. If needed, override the `mongoexhaust.wrapper` function.
- MongoExhaust respects references. If you provide your MongoEngine `Document` with a `_expand_fields` list containing `ReferenceField`s or `ListField` of `ReferenceField`s, these fields will be automatically expanded. For example:

        class SuperModel(Document):
            name = StringField()
            photo_shoots = ReferenceField(PhotoShoot)
            _expand_fields = ['photo_shoots']

- To turn off the automatic expansion of reference fields on a per-call basis, use `Document.to_bson(expand=false)`


## License

MIT License.
