#vedic_artical341
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Understanding 4-Bit Vedic Multiplier">
    <meta name="keywords" content="4-Bit Vedic Multiplier, Digital Design, VLSI, Multiplication, Vedic Mathematics">
    <meta name="author" content="Rohith Kumar">
    <title>4-Bit Vedic Multiplier - Explained</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 0;
            color: #333;
            background-color: #f4f4f9;
        }
        header {
            background: #007BFF;
            color: #fff;
            padding: 10px 20px;
            text-align: center;
        }
        main {
            padding: 20px;
            max-width: 800px;
            margin: auto;
        }
        section {
            margin-bottom: 20px;
        }
	h1{
            color: #FFFFFF;
        h1, h2, h3 {
            color: #007BFF;
        }
        footer {
            text-align: center;
            padding: 10px;
            background: #333;
            color: #fff;
            position: relative;
            bottom: 0;
        }
        .code {
            background: #eee;
            padding: 10px;
            border-radius: 5px;
            font-family: monospace;
            overflow-x: auto;
        }
    </style>
</head>
<body>
    <header>
        <h1>4-Bit Vedic Multiplier - Explained</h1>
    </header>
    <main>
        <section>
            <h2>Introduction</h2>
            <p>
                A <strong>4-bit Vedic Multiplier</strong> is a digital circuit designed using the ancient Vedic
                Mathematics technique called <em>Urdhva Tiryagbhyam</em> (Vertically and Crosswise). It is known
                for its simplicity, efficiency, and suitability for high-speed operations. This method allows
                multiplication of binary numbers with minimal delay and area.
            </p>
        </section>
        <section>
            <h2>Working Principle</h2>
            <p>
                The Urdhva Tiryagbhyam algorithm works by breaking down the multiplication process into smaller steps:
            </p>
            <ul>
                <li>Vertically multiply the least significant bits (LSBs).</li>
                <li>Cross-multiply and add intermediate results.</li>
                <li>Carry forward the values to the next step.</li>
            </ul>
        </section>
        <section>
            <h2>Design Structure</h2>
            <p>The 4-bit multiplier is implemented using smaller 2-bit multipliers, combined hierarchically:</p>
            <ul>
                <li>Inputs: Two 4-bit binary numbers, <code>A = A3A2A1A0</code> and <code>B = B3B2B1B0</code>.</li>
                <li>Output: An 8-bit binary product, <code>P = P7P6P5P4P3P2P1P0</code>.</li>
                <li>Components: Smaller 2-bit multipliers, adders, and logic gates.</li>
            </ul>
        </section>
        <section>
            <h2>Advantages</h2>
            <ul>
                <li><strong>High-Speed</strong>: Parallel computation of partial products.</li>
                <li><strong>Low Power</strong>: Efficient for mobile and embedded systems.</li>
                <li><strong>Compact</strong>: Requires less area compared to traditional multipliers.</li>
            </ul>
        </section>
        <section>
            <h2>Applications</h2>
            <ul>
                <li>Digital Signal Processing (DSP)</li>
                <li>High-Speed Arithmetic Units</li>
                <li>Cryptographic Systems</li>
            </ul>
        </section>
        <section>
            <h2>Sample Verilog Code</h2>
            <p>Below is a Verilog implementation of a 4-bit Vedic multiplier:</p>
            <div class="code">
<pre>
//
`timescale 1 ns/1 ns
//`include "andg.v"
module andg(y,a,b);
input a,b;
output y;
and a1(y,a,b);
endmodule

//`include "ha.v"
module hag(s,c,a,b);
input a,b;
output s,c;
xor a1(s,a,b);
and a2(c,a,b);
endmodule

//`include "fa.v"
module fag(s,c,a,b,cin);
input a,b,cin;
output s,c;
wire [3:1]w;
xor x1(w[1],a,b);
and x2(w[2],a,b);
xor x3(s,w[1],cin);
and x4(w[3],cin,w[1]);
or x5(c,w[2],w[3]);
endmodule


//vedic 4-bit
module vedic4bit(p,a,b);
input [3:0]a,b;
output [7:0]p;
wire [32:1]w;
andg z1(p[0],a[0],b[0]);// and block
andg z2(w[1],a[1],b[0]);
andg z3(w[2],a[0],b[1]);
hag z4(p[1],w[3],w[1],w[2]);  //1-1st ha adder
andg z5(w[4],a[2],b[0]);
andg z6(w[5],a[1],b[1]);
andg z7(w[6],a[0],b[2]);
fag z8(w[7],w[8],w[4],w[5],w[6]);  //1-1 full adder
hag z9(p[2],w[9],w[3],w[7]); //3-1 half addr
andg z10(w[10],a[3],b[0]); 
andg z11(w[11],a[2],b[1]);
andg z12(w[12],a[1],b[2]);
fag z13(w[13],w[14],w[10],w[11],w[12]); //1-2 full addr
andg z14(w[15],a[0],b[3]);
hag z15(w[16],w[17],w[13],w[15]); //2-1 ha
fag z16(p[3],w[18],w[16],w[8],w[9]);//3-1 fa
andg z17(w[19],a[3],b[1]); 
andg z19(w[20],a[2],b[2]);
andg z20(w[21],a[1],b[3]);
fag z21(w[22],w[23],w[20],w[21],w[19]);//1-3 full addr
hag z22(w[24],w[25],w[22],w[14]); //2-2 full addr
fag z23(p[4],w[26],w[18],w[17],w[24]);//3-2 full addr
andg z24(w[27],a[2],b[3]);
andg z25(w[28],a[3],b[2]);
fag z26(w[29],w[30],w[23],w[27],w[28]);  // 1-3 full adder
fag z27(p[5],w[31],w[26],w[25],w[29]); //3-3 full adder
andg z28(w[32],a[3],b[3]);
fag z29(p[6],p[7],w[31],w[30],w[32]);
endmodule

//Test Bench
module vedic4bit_tb;
reg [3:0]a,b;
wire [7:0]p;
vedic4bit aa1(p,a,b);
initial
repeat(100)
begin
a=$random;
b=$random;
#100;
end
endmodule
</pre>
            </div>
        </section>
        <section>
            <h2>Conclusion</h2>
            <p>
                The 4-bit Vedic Multiplier showcases the power of combining ancient mathematical principles with modern digital design techniques. It is an efficient and scalable solution for many applications requiring high-speed and low-power multiplication.
            </p>
        </section>
    </main>
    <footer>
        <p>© 2024 Rohith Kumar | All Rights Reserved</p>
    </footer>
</body>
</html>
