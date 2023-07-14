# 第2课 课后作业

## 第1题 Num2Bits

```
pragma circom 2.1.4;

template Num2Bits (nbits) {
    signal input in;
    signal output b[nbits];
    assert(in <= 2<<nbits);
    
    var sum = 0;
    var cur = 1;
    for(var i = 0; i < nbits; i++) {
        b[i] <-- (in >> i) & 1;
        0 === b[i] * (1 - b[i]);
        sum += cur * b[i];
        cur = cur * 2;
    }

    sum === in;
}

component main = Num2Bits(4);

/* INPUT = {
    "in": "9"
} */
```

## 第2题 IsZero
```
pragma circom 2.1.4;

template IsZero() {
    signal input in;
    signal output out;

    signal inv;
    inv <-- (in == 0) ? 1 : 1/in;
    out <== 1 - in * inv;
    0 === in * out;
}

component main = IsZero();

/* INPUT = {
    "in": "0"
} */
```

## 第3题 IsEqual
```
pragma circom 2.1.4;

template IsEqual() {
    signal input in[2];
    signal output out;

    component isZero = IsZero();
    isZero.in <== in[0] - in[1];
    out <== isZero.out;
}

component main = IsEqual();

/* INPUT = {
    "in": ["0", "1"]
} */
```

## 第4题 Selector
```
pragma circom 2.1.4;

template Selector(nChoices) {
    signal input in[nChoices];
    signal input index;
    signal output out;

    component isEqual[nChoices];
    var sum = 0;
    signal internalValues[nChoices];
    for(var i = 0; i < nChoices; i++) {
        isEqual[i] = IsEqual();
        isEqual[i].in[0] <== i;
        isEqual[i].in[1] <== index;
        internalValues[i] <== isEqual[i].out * in[i];
        sum += internalValues[i];
    }

    out <== sum;
}

component main = Selector(6);

/* INPUT = {
    "in": ["0", "1", "2", "3", "4", "5"],
    "index": "6"
} */
```

## 第5题 IsNegative
```
pragma circom 2.1.4;

include "./circomlib/compconstant.circom";

template IsNegative() {
    signal input in;
    signal output out;

    component n2b = Num2Bits(254);
    component comp = CompConstant(10944121435919637611123202872628637544274182200208017171849102093287904247808);

    n2b.in <== in;
    comp.in <== n2b.out;
    out <== comp.out;
}

component main = IsNegative();

/* INPUT = {
    "in": "10944121435919637611123202872628637544274182200208017171849102093287904248808"
} */
```

## 第6题 LessThan
```
pragma circom 2.1.4;

template LessThan(k) {
    signal input in[2];
    signal output out;
    assert(k <= 252);

    // use internal Num2Bits template
    component n2b = Num2Bits(k + 1);
    n2b.in <== (1 << k) + in[1] - in[0];
    out <== n2b.b[k]; 
}

component main = LessThan(3);

/* INPUT = {
    "in": ["1", "2"]
} */
```

## 第7题 IntergerDivide
```
pragma circom 2.1.4;

include "./circomlib/compconstant.circom";

template IntegerDivide(nbits) {
    assert(nbits <= 126);
    signal input dividend;
    signal input divisor;
    signal output remainder;
    signal output quotient;

    component ccs[3];
    component n2bs[3];
    for (var i = 0; i < 3; i++) {
        ccs[i] = CompConstant((1<<126)-1);
        n2bs[i] = Num2Bits(254);
    }

    quotient <-- dividend \ divisor;
    remainder <-- dividend % divisor;

    n2bs[0].in <== dividend;
    ccs[0].in <== n2bs[0].out;
    n2bs[1].in <== divisor;
    ccs[1].in <== n2bs[1].out;
    n2bs[2].in <== quotient;
    ccs[2].in <== n2bs[2].out;
    ccs[0].out === 0;
    ccs[1].out === 0;
    ccs[2].out === 0;

    component lt = LessThan(nbits);
    lt.in <== [remainder, divisor];
    lt.out === 1;

    dividend === quotient * divisor + remainder;
}

component main = IntegerDivide(10);

/* INPUT = {
    "dividend": "333",
    "divisor": "4"
} */
```

## 第8题 BubbleSort
```
pragma circom 2.1.4;

template Compare2(nbits) {
    signal input in[2];
    signal output min;
    signal output max;

    component lt = LessThan(nbits);
    lt.in <== in;
    min <== in[1] + (in[0] - in[1]) * lt.out;
    max <== in[0] + (in[1] - in[0]) * lt.out;
}

template BubbleMax(nbits, N, M) {
    signal input in[N];
    signal output out[N];
    assert(M > 0);
    assert(M <= N);

    for(var i=M; i<N; i++) {
        out[i] <== in[i];
    }

    component css[M-1];
    signal temps[M];
    temps[0] <-- in[0];
    for(var i=1; i<M; i++) {
        css[i-1] = Compare2(nbits);
        css[i-1].in <== [temps[i-1], in[i]];
        out[i-1] <== css[i-1].min;
        temps[i] <-- css[i-1].max;
    }
    out[M-1] <== temps[M-1];
}


template BubbleSort(nbits, N) {
    signal input in[N];
    signal output out[N];
    assert(N > 0);

    signal step[N];
    component bms[N];
    bms[0] = BubbleMax(nbits, N, N);
    bms[0].in <== in;
    for(var i = 1; i < N; i++) {
        bms[i] = BubbleMax(nbits, N, N-i);
        bms[i].in <== bms[i-1].out; 
    }
    out <== bms[N-1].out;
}

component main = BubbleSort(10, 2);

/* INPUT = {
    "in": ["999", "444"]
} */
```
