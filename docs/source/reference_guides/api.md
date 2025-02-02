# API

## Marks

pytask uses marks to attach additional information to task functions which is processed
by the host or by plugins. The following marks are available by default.

### Marks

```{eval-rst}
.. function:: pytask.mark.depends_on(objects: Any | Iterable[Any] | dict[Any, Any])

    Specify dependencies for a task.

    :type objects: Any | Iterable[Any] | dict[Any, Any]
    :param objects:
        Can be any valid Python object or an iterable of any Python objects. To be
        valid, it must be parsed by some hook implementation for the
        :func:`_pytask.hookspecs.pytask_collect_node` entry-point.
```

```{eval-rst}
.. function:: pytask.mark.parametrize(arg_names, arg_values, *, ids)

    Parametrize a task function.

    Parametrizing a task allows to execute the same task with different arguments.

    :type arg_names: str | list[str] | tuple[str, ...]
    :param arg_names:
        The names of the arguments which can either be given as a comma-separated
        string, a tuple of strings, or a list of strings.
    :type arg_values: Iterable[Sequence[Any] | Any]
    :param arg_values:
        The values which correspond to names in ``arg_names``. For one argument, it is a
        single iterable. For multiple argument names it is an iterable of iterables.
    :type ids: None | (Iterable[None | str | float | int | bool] | Callable[..., Any])
    :param ids:
        This argument can either be a list with ids or a function which is called with
        every value passed to the parametrized function.

        If you pass an iterable with ids, make sure to only use :obj:`bool`,
        :obj:`float`, :obj:`int`, or :obj:`str` as values which are used to create task
        ids like ``"task_dummpy.py::task_dummy[first_task_id]"``.

        If you pass a function, the function receives each value of the parametrization
        and may return a boolean, number, string or None. For the latter, the
        auto-generated value is used.
```

```{eval-rst}
.. function:: pytask.mark.persist()

    A marker for a task which should be peristed.
```

```{eval-rst}
.. function:: pytask.mark.produces(objects: Any | Iterable[Any] | dict[Any, Any])

    Specify products of a task.

    :type objects: Any | Iterable[Any] | dict[Any, Any]
    :param objects:
        Can be any valid Python object or an iterable of any Python objects. To be
        valid, it must be parsed by some hook implementation for the
        :func:`_pytask.hookspecs.pytask_collect_node` entry-point.
```

```{eval-rst}
.. function:: pytask.mark.skipif(condition: bool, *, reason: str)

    Skip a task based on a condition and provide a necessary reason.

    :param bool condition: A condition for when the task is skipped.
    :param str reason: A reason why the task is skipped.
```

```{eval-rst}
.. function:: pytask.mark.skip_ancestor_failed(reason: str = "No reason provided")

    An internal marker for a task which is skipped because an ancestor failed.

    :param str reason: A reason why the task is skipped.
```

```{eval-rst}
.. function:: pytask.mark.skip_unchanged()

    An internal marker for a task which is skipped because nothing has changed.

    :param str reason: A reason why the task is skipped.
```

```{eval-rst}
.. function:: pytask.mark.skip()

    Skip a task.
```

```{eval-rst}
.. function:: pytask.mark.task(name, *, id, kwargs)

    The task decorator allows to mark any task function regardless of its name as a task
    or assigns a new task name.

    It also allows to repeat tasks in for-loops by adding a specific ``id`` or keyword
    arguments via ``kwargs``.

    :type name: str | None
    :param name: The name of the task.
    :type id: str | None
    :param id:  An id for the task if it is part of a parametrization.
    :type kwargs: dict[Any, Any] | None
    :param kwargs:
        A dictionary containing keyword arguments which are passed to the task when it
        is executed.

```

```{eval-rst}
.. function:: pytask.mark.try_first

    Indicate that the task should be executed as soon as possible.

    This indicator is a soft measure to influence the execution order of pytask.

    .. important::

        This indicator is not intended for general use to influence the build order and
        to overcome misspecification of task dependencies and products.

        It should only be applied to situations where it is hard to define all
        dependencies and products and automatic inference may be incomplete like with
        pytask-latex and latex-dependency-scanner.

```

```{eval-rst}
.. function:: pytask.mark.try_last

    Indicate that the task should be executed as late as possible.

    This indicator is a soft measure to influence the execution order of pytask.

    .. important::

        This indicator is not intended for general use to influence the build order and
        to overcome misspecification of task dependencies and products.

        It should only be applied to situations where it is hard to define all
        dependencies and products and automatic inference may be incomplete like with
        pytask-latex and latex-dependency-scanner.
```

### Custom marks

Marks are created dynamically using the factory object {class}`pytask.mark` and applied
as a decorator.

For example:

```python
@pytask.mark.timeout(10, "slow", method="thread")
def task_function():
    ...
```

Will create and attach a {class}`Mark <pytask.Mark>` object to the collected
{class}`Task <pytask.Task>` to the `markers` attribute. The `mark` object will have the
following attributes:

```python
mark.args == (10, "slow")
mark.kwargs == {"method": "thread"}
```

Example for using multiple custom markers:

```python
@pytask.mark.timeout(10, "slow", method="thread")
@pytask.mark.slow
def task_function():
    ...
```

### Classes

```{eval-rst}
.. autoclass:: pytask.Mark
.. autoclass:: pytask.mark
.. autoclass:: pytask.MarkDecorator
.. autoclass:: pytask.MarkGenerator
```

### Functions to work with marks

```{eval-rst}
.. autofunction:: pytask.get_all_marks
.. autofunction:: pytask.get_marks
.. autofunction:: pytask.has_mark
.. autofunction:: pytask.remove_marks
.. autofunction:: pytask.set_marks
```

## Exceptions

Exceptions all inherit from

```{eval-rst}
.. autoclass:: pytask.PytaskError
```

The following exceptions can be used to interrupt pytask's flow, emit reduced tracebacks
and return the correct exit codes.

```{eval-rst}
.. autoclass:: pytask.CollectionError
.. autoclass:: pytask.ConfigurationError
.. autoclass:: pytask.ExecutionError
.. autoclass:: pytask.ResolvingDependenciesError
```

The remaining exceptions convey specific errors.

```{eval-rst}
.. autoclass:: pytask.NodeNotCollectedError
.. autoclass:: pytask.NodeNotFoundError
```

## General classes

```{eval-rst}
.. autoclass:: pytask.console
.. autoclass:: pytask.FilePathNode
.. autoclass:: pytask.MetaNode
.. autoclass:: pytask.Task
.. autoclass:: pytask.CollectionMetadata
.. autoclass:: pytask.CollectionReport
.. autoclass:: pytask.ExecutionReport
.. autoclass:: pytask.ResolvingDependenciesReport
.. autoclass:: pytask.Session
```

## General functions

```{eval-rst}
.. autofunction:: pytask.depends_on
.. autofunction:: pytask.produces
.. autofunction:: pytask.parse_nodes
.. autofunction:: pytask.check_for_optional_program
.. autofunction:: pytask.import_optional_dependency
```

## Outcomes

The exit code of pytask is determined by

```{eval-rst}
.. autoclass:: pytask.ExitCode
    :members:
    :member-order: bysource
```

Collected items can have the following outcomes

```{eval-rst}
.. autoclass:: pytask.CollectionOutcome
```

Tasks can have the following outcomes

```{eval-rst}
.. autoclass:: pytask.TaskOutcome
```

The following exceptions are used to abort the execution of a task with an appropriate
outcome.

```{eval-rst}
.. autoclass:: pytask.Exit
.. autoclass:: pytask.Persisted
.. autoclass:: pytask.Skipped
.. autoclass:: pytask.SkippedAncestorFailed
.. autoclass:: pytask.SkippedUnchanged
```

## Programmatic Interfaces

```{eval-rst}
.. autofunction:: pytask.build_dag
.. autofunction:: pytask.main
```

## Tracebacks

```{eval-rst}
.. autofunction:: pytask.format_exception_without_traceback
.. autofunction:: pytask.remove_internal_traceback_frames_from_exc_info
.. autofunction:: pytask.remove_traceback_from_exc_info
.. autofunction:: pytask.render_exc_info
```
