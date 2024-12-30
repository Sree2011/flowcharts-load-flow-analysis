<script type="module">
	import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11.4/dist/mermaid.esm.min.mjs';
	mermaid.initialize({
		startOnLoad: true,
		theme: 'light'
	});
</script>

# Line flows and losses


<pre class="mermaid">
graph TB
subgraph "main()"
    direction TB
    A1([Start]) --> B1[\Display 
    **Enter the no.of buses**\]
    B1 --> C1[/Input n/]
    C1 --> D1[["Initialise matrices V,I,y 
    with dimensions (n,n)"]]
    D1 --> E1[\Display
    **Enter 1 for impedance 
    and 2 for admittance**\]
    E1 --> G1[["Call 
    **get_input(n,V,I,y)**"]]
    G1 --> H1[/"store the result 
    into V,I and y"/]
    H1 --> I1[["Call 
    **calculate_line_flow_
    loss(n,V,I,y)**"]]
    I1 --> J1[/"store the result
    into S and SL"/]
    J1 --> K1[["Call 
    **display_output(n,V,I,S,SL)**"]]
    K1 --> M1([End])
end

subgraph "get_input(n,V,I,y)"
    direction TB
    A2([Start]) --> B2[for i from 0 to n-1]@{shape: hex}
    B2 --> C2[\"Display 
    **Enter voltage at bus i**"\]
    C2 --> D2[/"`Input 
    V[i,i]`"/]
    D2 --> E2[\"Display 
    **Enter current at bus i**"\]
    E2 --> F2[/"Input
    I[i,i]"/]
    F2 --> G2[" Display 
    **Enter 1 for impedance 
    and 2 for admittance**"]
    G2 --> H2[/Input choice/]
    H2 --> I2[choice]@{shape: diam}
    I2 --> |1| I3[for i from 0 to n-1]@{shape: hex}
    I3 --> I4[for j from 0 to n-1]@{shape: hex}
    I4 --> I5["Display 
    **Enter impedance between 
    bus i and j**"]@{shape: lean-left}
    I5 --> I6[/Input yij/]
    I6 --> I7["Assign reciprocal of 
    yij to y[i,j]"]
    I7 --> I8[Next iteration]
    I8 --> I3
    I2 --> |2| I9[for i from 0 to n-1]@{shape: hex}
    I9 --> I10[for j from 0 to n-1]@{shape: hex}
    I10 --> I11[\"Display 
    **Enter admittance between 
    bus i and j**"\]
    I11 --> I12[/"`Input 
    y[i,j]`"/]
    I12 --> I13[Next iteration]
    I13 --> I9

    I8 --> I14[Return 
    V,I,y]@{shape: stadium}
    I13 --> I14
end

subgraph calculate_lineflow_loss(n,V,I,y)

end
</pre>
