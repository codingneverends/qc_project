# Grover's Algo Implementaion

    Grover's algo can speed up unstructured data searching.
    Process involved are
        Oracle  creation and Amplitude Amplification.

### Problem
    Find |11> state from all 2 qubit states(|00>,|01>,|10>,|11>).

### Oracle
    Oracle can be used to chek wheather a value is target value or not , but it cannot find the target value 
    from range of values.

    Since we are finding |11> , we used CZ gate.So sign of |11> will be revesed.

        oracle = QuantumCircuit(2,name='Oracle')
        oracle.cz(1,0)
        oracle.to_gate()
        oracle.draw()

### Amplitude amplification
    It will increase the probability of target state.