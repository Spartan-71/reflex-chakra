---
components:
    - rc.Input
---

```python exec
import reflex_chakra as rc
import reflex as rx
```

# Input

The input component is used to receive text input from the user.

```python demo exec
class InputState(rx.State):
    text: str = "Type something..."

def basic_input_example():
    return rc.vstack(
        rc.text(InputState.text, color_scheme="green"),
        rc.input(value=InputState.text, on_change=InputState.set_text)
    )
```

"Behind the scene, the input component is implemented using debounced input to avoid sending individual state updates per character to the backend while the user is still typing.
This allows a state var to directly control the `value` prop from the backend without the user experiencing input lag.
For advanced use cases, you can tune the debounce delay by setting the `debounce_timeout` when creating the Input component.
You can find examples of how it is used in the [DebouncedInput]() component.

```python demo exec
class ClearInputState(rx.State):
    text: str

    def clear_text(self):
        self.text = ""


def clear_input_example():
    return rc.vstack(
        rc.text(ClearInputState.text),
        rc.input(
            on_change=ClearInputState.set_text,
            value=ClearInputState.text,
        ),
        rc.button("Clear", on_click=ClearInputState.clear_text),
    )
```

The input component can also use the `on_blur` event handler to only change the state when the user clicks away from the input.
This is useful for performance reasons, as the state will only be updated when the user is done typing.

```python demo exec
class InputBlurState(rx.State):
    text: str = "Type something..."

    def set_text(self, text: str):
        self.text = text.upper()


def input_blur_example():
    return rc.vstack(
        rc.text(InputBlurState.text),
        rc.input(placeholder="Type something...", on_blur=InputBlurState.set_text)
    )
```

You can change the type of input by using the `type_` prop.
For example you can create a password input or a date picker.

```python demo
rc.vstack(
    rc.input(type_="password"),
    rc.input(type_="date"),
)
```

We also provide a `rc.password` component as a shorthand for the password input.

```python demo
rc.password()
```

You can also use forms in combination with inputs.
This is useful for collecting multiple values with a single event handler and automatically supporting `Enter` key submit functionality that desktop users expect.

```python demo exec
class InputFormState(rx.State):
    form_data: dict = {}

    def handle_submit(self, form_data: dict):
        """Handle the form submit."""
        self.form_data = form_data
    
def input_form_example():
    return rc.vstack(
        rc.form(
            rc.vstack(
                rc.input(placeholder="First Name", id="first_name"),
                rc.input(placeholder="Last Name", id="last_name"),
                rc.button("Submit", type_="submit"),
            ),
            on_submit=InputFormState.handle_submit,
        ),
        rc.divider(),
        rc.heading("Results"),
        rc.text(InputFormState.form_data.to_string()),
    )
```
