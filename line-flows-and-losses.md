<script type="module">
	import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11.4/dist/mermaid.esm.min.mjs';
	mermaid.initialize({
		startOnLoad: true,
		theme: 'light'
	});
</script>

# Line flows and losses
<pre class="mermaid">
graph LR
    subgraph "Get Input"
        direction TB
        K([Start])
        K --> L[FOR i from 0 to n-1]@{shape: hex}
        L --> M["DISPLAY enter the 
        voltage at bus (i+1)"]@{shape: lean-left}
        M --> N["INPUT V[i, i]"]@{shape: lean-right}
        N --> O["DISPLAY enter the 
        current at bus (i+1)"]@{shape: lean-left}
        O --> P["INPUT I[i, i]"]@{shape: lean-right}
        P --> Q[End FOR]@{shape: hex}
        Q --> L
        Q --> R["DISPLAY 
        Enter 1 for impedance 
        and 2 for admittance"]@{shape: lean-left}
        R --> S[INPUT choice]@{shape: lean-right}
        S --> S1[Choice]@{shape: diam}
        S1 --> |1| T[FOR i from 0 to n-1]@{shape: hex}
        T --> U[FOR j from 0 to n-1]@{shape: hex}
        U --> V["DISPLAY 
        enter the impedance
        between bus (i+1) and (j+1)"]@{shape: lean-left}
        V --> W[INPUT yij]@{shape: lean-right}
        W --> X[["y[i, j] = 
        1 / yij"]]
        X --> Y[End FOR]@{shape: hex}
        Y --> U
        Y --> Z[End FOR]@{shape: hex}
        Z --> T
        S1 --> |2| BB[FOR i from 0 to n-1]@{shape: hex}
        BB --> CC[FOR j from 0 to n-1]@{shape: hex}
        CC --> DD["DISPLAY 
        enter the admittance 
        between bus (i+1) and (j+1)"]@{shape: lean-left}
        DD --> EE["INPUT y[i, j]"]@{shape: lean-right}
        EE --> FF[End FOR]@{shape: hex}
        FF --> CC
        FF --> GG[End FOR]@{shape: hex}
        GG --> BB
        GG --> II[/RETURN V,I,y\]
        Z --> II
        II --> JJ([End])
    end
</pre>

<pre class="mermaid">
graph LR
    subgraph "Main Program"
        direction TB
        A([Start])
        A --> B["DISPLAY 
        Enter the number of buses"]@{shape: lean-left}
        B --> C[INPUT n]@{shape: lean-right}
        C --> D[["INITIALISE matrices V, I, y 
        with dimensions (n, n)"]]
        D --> E["DISPLAY 
        Enter 1 for impedance 
        and 2 for admittance"]@{shape: lean-left}
        E --> F[INPUT choice]@{shape: lean-right}
        F --> G[["CALL get_input(n,V,I,y) and 
        store the result into V,I,y"]]
        G --> H[["CALL calculate_line_flow_
        loss(n,V,I,y) and store the 
        result into S and SL"]]
        H --> I[["CALL 
        display_output(n,V,I,S,SL)"]]
        I --> J([End])
    end
</pre>

<pre class="mermaid">
graph LR
    subgraph "Calculate Line Flows and Line Losses"
        direction TB
        KK([Start])
        KK --> LL[["INITIALISE MATRICES S,SL 
        with dimensions (n,n)"]]
        LL --> MM[FOR i from 0 to n-1]@{shape: hex}
        MM --> NN[FOR j from 0 to n-1]@{shape: hex}
        NN --> OO[i is not equal to j]@{shape: diam}
        OO --> |True| PP[["V[i, j] = V[i, i] - V[j, j]"]]
        PP --> QQ[["V[j, i] = V[j, j] - V[i, i]"]]
        QQ --> SS[End FOR]@{shape: hex}
        SS --> NN
        SS --> TT[End FOR]@{shape: hex}
        TT --> MM
        TT --> UU[FOR i from 0 to n-1]@{shape: hex}
        UU --> VV[FOR j from 0 to n-1]@{shape: hex}
        VV --> WW[IF i is not equal to j]@{shape: diam}
        WW --> XX["I[i, j] = y[i, j] *
        (V[i, i] - V[j, j])"]
        XX --> YY["I[j, i] = y[j, i] *
        (V[j, j] - V[i, i])"]
        YY --> AAA[End FOR]@{shape: hex}
        AAA --> VV
        AAA --> BBB[End FOR]@{shape: hex}
        BBB --> UU
        BBB --> CCC[FOR i from 0 to n-1]@{shape: hex}
        CCC --> DDD[FOR j from 0 to n-1]@{shape: hex}
        DDD --> EEE[["S[i, j] = V[i, j] *
        Conjugate(I[i, j])"]]
        EEE --> FFF[["S[j, i] = V[j, i] *
        Conjugate(I[j, i])"]]
        FFF --> GGG[["SL[i, j] = 
        S[i, j] + S[j, i]"]]
        GGG --> HHH[End FOR]@{shape: hex}
        HHH --> DDD
        HHH --> III[End FOR]@{shape: hex}
        III --> CCC
        III --> JJJ[/RETURN S,SL\]
        JJJ --> KKK([End])
    end
</pre>

<pre class="mermaid">
graph LR
    subgraph Display Line Flows and Losses
        direction TB
        LLL([Start])
        LLL --> MMM[[CREATE LIST data]]
        MMM --> NNN[FOR i from 0 to n-1]@{shape: hex}
        NNN --> OOO[FOR j from 0 to n-1]@{shape: hex}
        OOO --> PPP[["ADD 
        'Bus Pair (i+1)-(j+1), 
        Voltage V[i, j], 
        Current I[i, j], 
        Line Flow S[i, j], 
        Line Loss SL[i, j]' TO data"]]
        PPP --> QQQ[End FOR]@{shape: hex}
        QQQ --> OOO
        QQQ --> RRR[End FOR]@{shape: hex}
        RRR --> NNN
        RRR --> SSS[["DECLARE 
        headers = ['Bus Pair', 
        'Voltage', 
        'Current', 
        'Line Flow', 
        'Line Loss']"]]
        SSS --> TTT[FOR i in headers]@{shape: hex}
        TTT --> UUU[DISPLAY i, end with space]@{shape: lean-left}
        UUU --> VVV[End FOR]@{shape: hex}
        VVV --> TTT
        VVV --> WWW[DISPLAY newline]@{shape: lean-left}
        WWW --> XXX[FOR i in data]@{shape: hex}
        XXX --> YYY[DISPLAY i]@{shape: lean-left}
        YYY --> ZZZ[End FOR]@{shape: hex}
        ZZZ --> XXX
        ZZZ --> AAAA([End])
    end
</pre>