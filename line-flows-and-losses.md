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
    classDef bigFontSize fill:#f9f,stroke:#333,stroke-width:10px,font-size:100px;
    
    
    subgraph "Get Input"
        direction LR
        K[Start]:::bigFontSize
        K --> L[FOR i from 0 to n-1]:::bigFontSize
        L --> M["DISPLAY enter the voltage at bus (i+1)"]:::bigFontSize
        M --> N["INPUT V[i, i]"]:::bigFontSize
        N --> O["DISPLAY enter the current at bus (i+1)"]:::bigFontSize
        O --> P["INPUT I[i, i]"]:::bigFontSize
        P --> Q[End FOR]:::bigFontSize
        Q --> R["DISPLAY Enter 1 for impedance and 2 for admittance"]:::bigFontSize
        R --> S[INPUT choice]:::bigFontSize
        S --> |choice == 1| T[FOR i from 0 to n-1]:::bigFontSize
        T --> U[FOR j from 0 to n-1]:::bigFontSize
        U --> V["DISPLAY enter the impedance between bus (i+1) and (j+1)"]:::bigFontSize
        V --> W[INPUT yij]:::bigFontSize
        W --> X["y[i, j] = 1 / yij"]:::bigFontSize
        X --> Y[End FOR]:::bigFontSize
        Y --> Z[End FOR]:::bigFontSize
        Z --> AA[End IF]:::bigFontSize
        S --> |choice == 2| BB[FOR i from 0 to n-1]:::bigFontSize
        BB --> CC[FOR j from 0 to n-1]:::bigFontSize
        CC --> DD["DISPLAY enter the admittance between bus (i+1) and (j+1)"]:::bigFontSize
        DD --> EE["INPUT y[i, j]"]:::bigFontSize
        EE --> FF[End FOR]:::bigFontSize
        FF --> GG[End FOR]:::bigFontSize
        GG --> HH[End IF]:::bigFontSize
        HH --> II[RETURN V,I,y]:::bigFontSize
        II --> JJ[End]:::bigFontSize
    end

    subgraph "Main Program"
        direction LR
        A[Start]:::bigFontSize
        A --> B["DISPLAY Enter the number of buses"]:::bigFontSize
        B --> C[INPUT n]:::bigFontSize
        C --> D["INITIALISE matrices V, I, y with dimensions (n, n)"]:::bigFontSize
        D --> E["DISPLAY Enter 1 for impedance and 2 for admittance"]:::bigFontSize
        E --> F[INPUT choice]:::bigFontSize
        F --> G["CALL get_input(n,V,I,y) and store the result into V,I,y"]:::bigFontSize
        G --> H["CALL calculate_line_flow_loss(n,V,I,y) and store the result into S and SL"]:::bigFontSize
        H --> I["CALL display_output(n,V,I,S,SL)"]:::bigFontSize
        I --> J[End]:::bigFontSize
    end
</pre>

<pre class="mermaid">
graph LR
    subgraph Display Line Flows and Line Losses
        direction LR
        LLL[Start]
        LLL --> MMM[CREATE LIST data]
        MMM --> NNN[FOR i from 0 to n-1]
        NNN --> OOO[FOR j from 0 to n-1]
        OOO --> PPP["ADD Bus Pair (i+1)-(j+1), Voltage V[i, j], Current I[i, j], Line Flow S[i, j], Line Loss SL[i, j] TO data"]
        PPP --> QQQ[End FOR]
        QQQ --> RRR[End FOR]
        RRR --> SSS["DECLARE headers = ['Bus Pair', 'Voltage', 'Current', 'Line Flow', 'Line Loss']"]
        SSS --> TTT[FOR i in headers]
        TTT --> UUU[DISPLAY i, end with space]
        UUU --> VVV[End FOR]
        VVV --> WWW[DISPLAY newline]
        WWW --> XXX[FOR i in data]
        XXX --> YYY[DISPLAY i]
        YYY --> ZZZ[End FOR]
        ZZZ --> AAAA[End]
    end
    subgraph Calculate Line Flows and Line Losses
        direction LR
        KK[Start]
        KK --> LL["INITIALISE MATRICES S,SL with dimensions (n,n)"]
        LL --> MM[FOR i from 0 to n-1]
        MM --> NN[FOR j from 0 to n-1]
        NN --> OO[IF i is not equal to j]
        OO --> PP["V[i, j] = V[i, i] - V[j, j]"]
        PP --> QQ["V[j, i] = V[j, j] - V[i, i]"]
        QQ --> RR[End IF]
        RR --> SS[End FOR]
        SS --> TT[End FOR]
        TT --> UU[FOR i from 0 to n-1]
        UU --> VV[FOR j from 0 to n-1]
        VV --> WW[IF i is not equal to j]
        WW --> XX["I[i, j] = y[i, j] * (V[i, i] - V[j, j])"]
        XX --> YY["I[j, i] = y[j, i] * (V[j, j] - V[i, i])"]
        YY --> ZZ[End IF]
        ZZ --> AAA[End FOR]
        AAA --> BBB[End FOR]
        BBB --> CCC[FOR i from 0 to n-1]
        CCC --> DDD[FOR j from 0 to n-1]
        DDD --> EEE["S[i, j] = V[i, j] * Conjugate(I[i, j])"]
        EEE --> FFF["S[j, i] = V[j, i] * Conjugate(I[j, i])"]
        FFF --> GGG["SL[i, j] = S[i, j] + S[j, i]"]
        GGG --> HHH[End FOR]
        HHH --> III[End FOR]
        III --> JJJ[RETURN S,SL]
        JJJ --> KKK[End]
    end

</pre>