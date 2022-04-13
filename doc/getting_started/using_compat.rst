~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Using compatibility module
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Compat is run automatically at the start of tarantool session. You can access it
as ``tarantool.compat`` or ``compat``. It lists all compatibility options
controlled by compat:

.. code-block:: tarantoolsession

        tarantool> compat
        ---
        - json_escape_forward_slash:
                value: true
                new: false
                doc: https://github.com/tarantool/tarantool/wiki/compat_json_escape_forward_slash
                brief: <...>
                old: true
                default: true
          option_2:
                value: true
                new: true
                doc: https://github.com/tarantool/tarantool/wiki/option_2
                brief: <...>
                old: false
                default: true
        ...

#. To change options you can do it directly:

   .. code-block:: tarantoolsession

        tarantool> compat.json_escape_forward_slash = 'old'
        ---
        ...

   or pass a list of such options

   .. code-block:: tarantoolsession

        tarantool> compat{json_escape_forward_slash = 'old', option_2 = false}
        ---
        ...

   all options can be set as 'old'/'new' according to such values in table or directly
   true/false.

#. You can check if a specific option has been set correctly:

   .. code-block:: tarantoolsession

        tarantool> compat.json_escape_forward_slash
        ---
        - value: true
          new: false
          doc: https://github.com/tarantool/tarantool/wiki/compat_json_escape_forward_slash
          brief: <...>
          old: true
          default: true
        ...

   or list all options:

   .. code-block:: tarantoolsession

        tarantool> compat
        ---
        - json_escape_forward_slash:
            value: true
            new: false
            doc: https://github.com/tarantool/tarantool/wiki/compat_json_escape_forward_slash
            brief: <...>
            old: true
            default: true
          option_2:
            value: false
            new: true
            doc: https://github.com/tarantool/tarantool/wiki/option_2
            brief: <...>
            old: false
            default: true
        ...

#. You can set a specific option to default:

   .. code-block:: tarantoolsession

        tarantool> compat.restore({'option_2'})
        ---
        ...

        tarantool> compat.option_2
        ---
        - value: true
          new: true
          doc: https://github.com/tarantool/tarantool/wiki/option_2
          brief: <...>
          old: false
          default: true
        ...

   or restore default for every option:

   .. code-block:: tarantoolsession

        tarantool> compat.reset()
        ---
        ...

#. To list all unselected options, run:

   .. code-block:: tarantoolsession

        tarantool> compat.candidates()
        ---
        - json_escape_forward_slash:
            value: true
            new: false
            doc: https://github.com/tarantool/tarantool/wiki/compat_json_escape_forward_slash
            brief: <...>
            old: true
            default: true
          option_2:
            value: true
            new: true
            doc: https://github.com/tarantool/tarantool/wiki/option_2
            brief: <...>
            old: false
            default: true
        ...

#. To get a command that can set up compat with the same options as current, run:

   .. code-block:: tarantoolsession

        tarantool> compat{json_escape_forward_slash = 'new', option_2 = 'new'}
        ---
        ...

        tarantool> compat.dump()
        ---
        - require('tarantool').compat({json_escape_forward_slash = false, option_2 = true})
        ...

