<script type="module">
	import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
	mermaid.initialize({
		startOnLoad: true,
		theme: 'light'
	});
</script>

# Formation of bus admittance matrix

<pre class="mermaid">
graph TB
    subgraph "**get_input(choice,n)**"
        direction TB
        V([Start]) --> K2{Choice}
        K2 -- 1 --> L1{For each bus i}
        L1 --> L2{For each bus j}
        L2 --> M1[/Take impedance 
        as input/]
        M1 --> M2[[Calculate admittance]]
        M2 --> M3[/store into y/]
        M3 --> L1
        M3 --> Q1([Return y to main])
        K2 -- 2 --> O1{For each bus i}
        O1 --> O2{For each bus j}
        O2 --> P1[/Take admittance as input/]
        P1 --> P2[/store into y/]
        P2 --> O1
        P2 --> Q1
    end
    subgraph main["main()"]
        direction TB
        A([Start]) --> B[\Display 
        'Enter the number of buses'\]
        B --> B1[/Input n/]
        B1 --> C[[Initialize Ybus 
        matrix with 0+0j]]
        C --> D[[Initialize y 
        matrix with 0+0j]]
        D --> E[\Display 
        'Enter 1 for impedance 
        and 2 for admittance'\]
        E --> E1[/Input choice/]
        E1 --> F{Choice}
        F -- 1 --> G[["Call 
        get_input(choice, n) 
        to get admittance matrix 
        from impedances"]]
        F -- 2 --> H[["Call 
        get_input(choice, n) 
        to get admittance matrix 
        from admittances"]]
        F -- Else --> AB[\Display 
        'Invalid Input'\]
        G --> I[["Call 
        calculate_matrix(y, n)"]]
        H --> I
        I --> J[["Call 
        display_matrix(Ybus)"]]
        J --> K([End])
        AB --> K
    end
</pre>

<pre class="mermaid">
graph TB
    subgraph "display_matrix(ybus,n)"
        direction TB
        X([Start]) --> X1[\Print 
        Bus Admittance Matrix\]
        X1 --> Y1{For each row in Ybus}
        Y1 --> Z1{For each element in row}
        Z1 --> AA1[\Print each element\]
        AA1 --> AB1[\Print new line\]
        Z1 --> Z2[[Next Iteration]]
        Z2 --> Y1
        Y1 --> W2([Return Ybus to main])
    end
    
    subgraph "calculate_matrix(y,n)"
        direction TB
        R([Start]) --> R1{For each bus i and j}
        R1 --> S1{i == j}
        S1 -- Yes --> T1[[Calculate 
        diagonal elements]]
        S1 -- No --> V1[[Calculate 
        off-diagonal elements]]
        V1 --> U1[[Next Iteration]]
        U1 --> R1
        U1 --> W1([Return Ybus to main])
    end
</pre>