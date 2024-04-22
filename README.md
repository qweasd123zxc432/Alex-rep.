class TuringMachine:
    def __init__(self, tape, initial_state, transition_function, final_states):
        self.tape = tape
        self.head_position = 0
        self.current_state = initial_state
        self.transition_function = transition_function
        self.final_states = final_states

    def step(self):
        current_symbol = self.tape[self.head_position]
        if (self.current_state, current_symbol) in self.transition_function:
            new_state, new_symbol, direction = self.transition_function[(self.current_state, current_symbol)]
            self.tape = self.tape[:self.head_position] + new_symbol + self.tape[self.head_position + 1:]
            if direction == 'R':
                self.head_position += 1
            elif direction == 'L':
                self.head_position -= 1
            self.current_state = new_state
        else:
            print("No transition defined for current state and symbol.")

    def run(self):
        while self.current_state not in self.final_states:
            self.step()
        print("Final tape:", self.tape)

# Пример использования:
transition_function = {('q0', '0'): ('q1', '1', 'R'),
                       ('q1', '1'): ('q0', '0', 'L')}
final_states = {'q0'}
tm = TuringMachine('0001000', 'q0', transition_function, final_states)
tm.run()
