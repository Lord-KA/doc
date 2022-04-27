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

   all options can be set as 'old'/'new' according to such values in the table or directly
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

#. You can also add options during runtime with add_options that requires list
   of tables with the following:
    - name (string)
    - options table (see src/lua/compat.lua:options_format; default may also be
      'new'/'old')
    - postaction function (arg - boolean as if behavior is on or off, changes the
      behavior accordingly)

   .. code-block:: tarantoolsession

        tarantool> c.add_options{{'option_1',
                                  {
                                     old = true,
                                     new = false,
                                     default = false,
                                     frozen = false,
                                     brief = "This is a brief message explaining why the option is here and what it changes.",
                                     doc = "This is a link to Tarantool wiki page with more details."
                                  },
                                  function() print("option_1 postaction was called!") end}}
        option_1 postaction was called!
        ---
        ...

   Note that postaction of a new option is called with its defaults right after
   it is added (before the next one in the list).

#. To add option to compat src directly, add table with option description (same
   as for add_options()) and function to postaction table. Every build-in option
   must have a Tarantool wiki page with detailed description, list of
   compatibility problems and some tips on detecting problems in codebase.
   For example, see json_escape_forward_slash patch (gh-6200).

#. Make sure to set up compat if needed at the very beginning of a
   program or module, right after the require calls.
