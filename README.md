# CS_lab9
CSRF Protection
===============

Any view using :class:`~flask_wtf.FlaskForm` to process the request is already
getting CSRF protection. If you have views that don't use ``FlaskForm`` or make
AJAX requests, use the provided CSRF extension to protect those requests as
well.

Setup
-----

To enable CSRF protection globally for a Flask app, register the
:class:`CSRFProtect` extension. ::

    from flask_wtf.csrf import CSRFProtect

    csrf = CSRFProtect(app)

Like other Flask extensions, you can apply it lazily::

    csrf = CSRFProtect()

    def create_app():
        app = Flask(__name__)
        csrf.init_app(app)
 HTML Forms
----------

When using a ``FlaskForm``, render the form's CSRF field like normal.

.. sourcecode:: html+jinja

    <form method="post">
        {{ form.csrf_token }}
    </form>

If the template doesn't use a ``FlaskForm``, render a hidden input with the
token in the form.

.. sourcecode:: html+jinja

    <form method="post">
        <input type="hidden" name="csrf_token" value="{{ csrf_token() }}"/>
    </form>

JavaScript Requests
-------------------

When sending an AJAX request, add the ``X-CSRFToken`` header to it.
For example, in jQuery you can configure all requests to send the token.

.. sourcecode:: html+jinja

    <script type="text/javascript">
        var csrf_token = "{{ csrf_token() }}";

        $.ajaxSetup({
            beforeSend: function(xhr, settings) {
                if (!/^(GET|HEAD|OPTIONS|TRACE)$/i.test(settings.type) && !this.crossDomain) {
                    xhr.setRequestHeader("X-CSRFToken", csrf_token);
                }
            }
        });
    </script>
