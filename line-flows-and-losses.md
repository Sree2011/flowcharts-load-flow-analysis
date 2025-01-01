<script type="module">
	import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11.4/dist/mermaid.esm.min.mjs';
	mermaid.initialize({
		startOnLoad: true,
		theme: 'light'
	});
</script>

# Line flows and losses
<pre>
graph TD
    subgraph "Main Program"
        A[Start]
        A --> B[DISPLAY "Enter the number of buses"]
        B --> C[INPUT n]
        C --> D["INITIALISE matrices V, I, y with dimensions (n, n)"]
        D --> E[DISPLAY "Enter 1 for impedance and 2 for admittance"]
        E --> F[INPUT choice]
        F --> G["V,I,y = get_input(n,V,I,y)"]
        G --> H["S,SL = calculate_line_flow_loss(n,V,I,y)"]
        H --> I["display_output(n,V,I,S,SL)"]
        I --> J[End]
    end

    subgraph "Get Input"
        K[Start]
        K --> L[FOR i from 0 to n-1]
        L --> M[DISPLAY "enter the voltage at bus (i+1)"]
        M --> N["INPUT V[i, i]"]
        N --> O[DISPLAY "enter the current at bus (i+1)"]
        O --> P["INPUT I[i, i]"]
        P --> Q[End FOR]
        Q --> R["DISPLAY Enter 1 for impedance and 2 for admittance"]
        R --> S[INPUT choice]
        S --> |choice == 1| T[FOR i from 0 to n-1]
        T --> U[FOR j from 0 to n-1]
        U --> V["DISPLAY enter the impedance between bus (i+1) and (j+1)"]
        V --> W[INPUT yij]
        W --> X[y[i, j] = 1 / yij]
        X --> Y[End FOR]
        Y --> Z[End FOR]
        Z --> AA[End IF]
        S --> |choice == 2| BB[FOR i from 0 to n-1]
        BB --> CC[FOR j from 0 to n-1]
        CC --> DD[DISPLAY "enter the admittance between bus (i+1) and (j+1)"]
        DD --> EE["INPUT y[i, j]"]
        EE --> FF[End FOR]
        FF --> GG[End FOR]
        GG --> HH[End IF]
        HH --> II[RETURN V,I,y]
        II --> JJ[End]
    end
</pre>

<pre>
graph TB
    subgraph Calculate Line Flows and Line Losses
        KK[Start]
        KK --> LL[INITIALISE MATRICES S,SL with dimensions (n,n)]
        LL --> MM[FOR i from 0 to n-1]
        MM --> NN[FOR j from 0 to n-1]
        NN --> OO[IF i is not equal to j]
        OO --> PP[V[i, j] = V[i, i] - V[j, j]]
        PP --> QQ[V[j, i] = V[j, j] - V[i, i]]
        QQ --> RR[End IF]
        RR --> SS[End FOR]
        SS --> TT[End FOR]
        TT --> UU[FOR i from 0 to n-1]
        UU --> VV[FOR j from 0 to n-1]
        VV --> WW[IF i is not equal to j]
        WW --> XX[I[i, j] = y[i, j] * (V[i, i] - V[j, j])]
        XX --> YY[I[j, i] = y[j, i] * (V[j, j] - V[i, i])]
        YY --> ZZ[End IF]
        ZZ --> AAA[End FOR]
        AAA --> BBB[End FOR]
        BBB --> CCC[FOR i from 0 to n-1]
        CCC --> DDD[FOR j from 0 to n-1]
        DDD --> EEE[S[i, j] = V[i, j] * Conjugate(I[i, j])]
        EEE --> FFF[S[j, i] = V[j, i] * Conjugate(I[j, i])]
        FFF --> GGG[SL[i, j] = S[i, j] + S[j, i]]
        GGG --> HHH[End FOR]
        HHH --> III[End FOR]
        III --> JJJ[RETURN S,SL]
        JJJ --> KKK[End]
    end

    subgraph Display Line Flows and Line Losses
        LLL[Start]
        LLL --> MMM[CREATE LIST data]
        MMM --> NNN[FOR i from 0 to n-1]
        NNN --> OOO[FOR j from 0 to n-1]
        OOO --> PPP[ADD "Bus Pair (i+1)-(j+1), Voltage V[i, j], Current I[i, j], Line Flow S[i, j], Line Loss SL[i, j]" TO data]
        PPP --> QQQ[End FOR]
        QQQ --> RRR[End FOR]
        RRR --> SSS[DECLARE headers = ["Bus Pair", "Voltage", "Current", "Line Flow", "Line Loss"]]
        SSS --> TTT[FOR i in headers]
        TTT --> UUU[DISPLAY i, end with space]
        UUU --> VVV[End FOR]
        VVV --> WWW[DISPLAY newline]
        WWW --> XXX[FOR i in data]
        XXX --> YYY[DISPLAY i]
        YYY --> ZZZ[End FOR]
        ZZZ --> AAAA[End]
    end
</pre>