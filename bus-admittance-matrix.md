<script type="module">
	import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
	mermaid.initialize({
		startOnLoad: true,
		theme: 'light'
	});
</script>

<pre class="mermaid">
graph TB
    subgraph "display_matrix(ybus,n)"
        direction LR
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
        direction LR
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

    subgraph "get_input(choice,n)"
        direction LR
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
        direction LR
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

   

   classDef input fill:#98F5F9,stroke:#333,stroke-width:2px,text-align:center;
   classDef process fill:#7DDA58,stroke:#333,stroke-width:2px,text-align:center;
   classDef output fill:#FE9900,stroke:#333,stroke-width:2px,text-align:center;
   classDef loop fill:#AD840E,stroke:#333,stroke-width:2px,text-align:center;
   classDef decision fill:#BFD641,stroke:#333,stroke-width:2px,text-align:center;
   classDef startEnd fill:#BA62D1,stroke:#333,stroke-width:2px,text-align:center;


   class B1,E1,M1,P1,M3,P2 input
   class C,D,G,H,I,J,M2,T1,V1,U1,Z2 process
   class B,E,AB,X1,Z1,AA1,AB1 output
   class F,K2,S1 decision
   class A,K,V,Q1,W1,W2,R,X,Z3 startEnd
   class L1,L2,O1,O2,R1,Y1,Z1 loop
</pre>