class InvalidFormatError(Exception):
    pass


class UnknownVariableError(Exception):
    pass


class DivisionByZeroError(Exception):
    pass


class UnsupportedOperatorError(Exception):
    pass


variables = {}


def get_value(value):
    try:
        return float(value)
    except ValueError:

        if value not in variables:
            raise UnknownVariableError(
                "Unknown variable"
            )

        return variables[value]


while True:

    line = input().strip()

    if line.lower() == "quit":
        break

    try:

        # Assignment
        if "=" in line:

            parts = line.split("=")

            if len(parts) != 2:
                raise InvalidFormatError(
                    "Invalid assignment"
                )

            name = parts[0].strip()
            value = parts[1].strip()

            if not name.isidentifier():
                raise InvalidFormatError(
                    "Invalid variable name"
                )

            variables[name] = get_value(value)

            continue

        parts = line.split()

        if len(parts) != 3:
            raise InvalidFormatError(
                "Expected: operand operator operand"
            )

        left, operator, right = parts

        if operator not in ["+", "-", "*", "/", "%"]:
            raise UnsupportedOperatorError(
                "Unsupported operator"
            )

        a = get_value(left)
        b = get_value(right)

        if operator == "/" and b == 0:
            raise DivisionByZeroError()

        if operator == "%" and b == 0:
            raise DivisionByZeroError()

        if operator == "+":
            result = a + b

        elif operator == "-":
            result = a - b

        elif operator == "*":
            result = a * b

        elif operator == "/":
            result = a / b

        else:
            result = a % b

        if result.is_integer():
            print(int(result))
        else:
            print(result)

    except (
        InvalidFormatError,
        UnknownVariableError,
        DivisionByZeroError,
        UnsupportedOperatorError
    ) as e:

        print(type(e).__name__)
