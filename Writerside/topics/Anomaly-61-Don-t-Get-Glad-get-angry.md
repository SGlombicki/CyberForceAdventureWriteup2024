# Anomaly 61 - Don&apos;t Get Glad, get angry

This challenge has repeated hints to use a binary analysis tool called angr. And we can see why once we start to decompile the binary.
![image_48.png](image_48.png)
We have repeated functions check each line and it will take forever to do by hand. So we can write a program to check for us.

```Python
import angr
import claripy

binary_path = './61/goodluck'

input_length = 1

proj = angr.Project(binary_path, auto_load_libs=False)
state = proj.factory.entry_state()

input_chars = [claripy.BVS('input_char_{}'.format(i), 8) for i in range(input_length)]
input_string = claripy.Concat(*input_chars + [claripy.BVV(b'\n')])

for k in input_chars:
    state.solver.add(k >= 0x20)
    state.solver.add(k <= 0x7e)

state.posix.stdin.write(input_string)
state.posix.stdin.seek(0)

simgr = proj.factory.simulation_manager(state)

def is_successful(state):
    return b'Next character' in state.posix.dumps(1)  # Adjust this according to the actual success message

def should_abort(state):
    return b'Wrong character' in state.posix.dumps(1)  # Adjust this according to the actual failure message

simgr.explore(find=is_successful, avoid=should_abort)

if simgr.found:
    found_state = simgr.found[0]
    solution = found_state.solver.eval(input_string, cast_to=bytes)
    print(f"Correct character: {solution}")
else:
    print("No valid character found.")

```
Output being stopped when i saw the flag
```code
skflcMB4zxefJCb9WOqn___flag{CongratulationsYouManagedToSolveTheSuperLongFlag_d64c332fa85489f770a9e59c99f66824}\n___...
```