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

<div style="display:flex">
<img src='images/czgate_py.png' width='100px'>
<img src='images/czgate.png' width='100px' style='margin-left:20px'>
</div>

### Amplitude amplification
It will increase the probability of target state.
2S>&lt;S - I is the reflection operator.
Grover's Diffusion is reflection operator sandwitched in hadamard (H 2S>&lt;S - I H ).

### Geometrical Intrepretation
There is a simple geometrical version of explanation is there . Let's go with that.
Let |w> denote winning state , |s> denote super position state.
|s'> denote state perpendicular to |w> in a way |w> component removed from |s>.

In our case 

        |w>  = [0,0,0,1]
        |s>  = (1/2)*[1,1,1,1]
        |s'> = (1/√3)*[1,1,1,0]
        <w|s'> = 0 , means |w> ans |s'> are perpendicular

Initial state.

<img src='images/grover_step1.jpg'>

Going through Oracle<br>
Oracle will flip amplitude of winning state(|w>) here |11>.

<img src='images/grover_step2.jpg'>

After Grover's  Diffusion<br>
The |s> formed after Oracle treatment is reflected along inital |s> , making it closer towards winning state.

<img src='images/grover_step3.jpg'>

Inorder to find reflection , one way of thinking is adding a negative phase π to all except |11>.

        diffusion = QuantumCircuit(2,name="Grover's diffusion")
        diffusion.h([0,1])
        diffusion.z([0,1])
        diffusion.cz(0,1)
        diffusion.to_gate()
        diffusion.h([0,1])
        diffusion.to_gate()
        diffusion.draw()
<div style="display:flex">
<img src='images/diffusion_py.png' width='200px'>
<img src='images/diffusion.png' width='200px' style="margin-left:20px">
</div>

### Grover's Iter
Both Oracle and Diffusion together is groover's iter.Probabilty of winning state(|w>) will be max after √N iterations.

        grover_iter = QuantumCircuit(2,name="Grover's Iter")
        grover_iter.append(oracle,[0,1])
        grover_iter.append(diffusion,[0,1])
        grover_iter.draw()

<img src='images/iter.png' width='300px'>
<img src='images/iter_py.png' width='300px'>

### Implmenting
Since we used 2 qubit system. One iteration is enough.
        
        grover_circ = QuantumCircuit(2,2)
        grover_circ.h([1,0])
        grover_circ.append(grover_iter,[0,1])
        grover_circ.measure([0,1],[0,1])
        grover_circ.draw()

<img src='images/circ.png' width='400px'>
<img src='images/circ_py.png' width='400px'>

Result in qasm_simulator<br>
All measurement falls to winning state |11>.
<div style="width:100%;background:'white'">
<img src='images/qasm.png'style="background-color:'white'">
</div>
Result in ibm_oslo -- IBM Quatum Computer<br>
Little deveations because of quatum noise.
<div style="width:100%;background:'white'">
<img src='images/oslo.png'style="background-color:'white'">
</div>