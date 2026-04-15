<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Darun's Advanced Calculator - Fully Working</title>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(135deg, #0a0a0a 0%, #1a1a2e 50%, #16213e 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 2rem;
            color: #fff;
        }

        .calculator-container {
            background: rgba(255, 255, 255, 0.03);
            backdrop-filter: blur(25px);
            border-radius: 30px;
            border: 1px solid rgba(0, 212, 255, 0.3);
            box-shadow: 
                0 25px 50px rgba(0, 0, 0, 0.5),
                0 0 100px rgba(0, 212, 255, 0.2);
            padding: 2.5rem;
            max-width: 450px;
            width: 100%;
            position: relative;
        }

        .header {
            text-align: center;
            margin-bottom: 2rem;
        }

        .title {
            font-family: 'Orbitron', monospace;
            font-size: 2rem;
            font-weight: 900;
            background: linear-gradient(45deg, #00d4ff, #00ff88, #ff6b6b);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 0.5rem;
        }

        .subtitle {
            color: #aaa;
            font-size: 0.95rem;
        }

        .display {
            background: rgba(0, 0, 0, 0.8);
            border: 2px solid rgba(0, 212, 255, 0.5);
            border-radius: 20px;
            padding: 2rem 1.5rem;
            margin-bottom: 2rem;
            min-height: 120px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            box-shadow: inset 0 5px 20px rgba(0, 0, 0, 0.5);
        }

        .display-input {
            font-family: 'Orbitron', monospace;
            font-size: 2.5rem;
            font-weight: 700;
            color: #00ff88;
            text-align: right;
            min-height: 60px;
            letter-spacing: 1px;
            word-break: break-all;
        }

        .display-output {
            font-family: 'Orbitron', monospace;
            font-size: 1.2rem;
            color: #aaa;
            text-align: right;
            min-height: 30px;
            opacity: 0.8;
        }

        .buttons-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1.2rem;
        }

        .btn {
            height: 70px;
            border: none;
            border-radius: 18px;
            font-size: 1.4rem;
            font-weight: 600;
            font-family: 'Orbitron', monospace;
            cursor: pointer;
            transition: all 0.2s ease;
            position: relative;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .btn:active {
            transform: scale(0.95);
        }

        /* Button Styles */
        .btn-number {
            background: linear-gradient(135deg, rgba(255,255,255,0.2), rgba(255,255,255,0.05));
            color: #fff;
            border: 1px solid rgba(255,255,255,0.2);
            backdrop-filter: blur(10px);
        }

        .btn-number:hover {
            background: linear-gradient(135deg, rgba(255,255,255,0.3), rgba(255,255,255,0.1));
            box-shadow: 0 8px 25px rgba(255,255,255,0.2);
        }

        .btn-operator {
            background: linear-gradient(135deg, #ff6b6b, #ff8e8e);
            color: #fff;
            font-size: 1.6rem;
            font-weight: 700;
        }

        .btn-operator:hover {
            background: linear-gradient(135deg, #ff5252, #ff6b6b);
            box-shadow: 0 10px 30px rgba(255,107,107,0.5);
            transform: translateY(-2px);
        }

        .btn-equals {
            background: linear-gradient(135deg, #00ff88, #32ffaa);
            color: #000;
            font-size: 1.8rem;
            font-weight: 900;
            grid-column: span 2;
        }

        .btn-equals:hover {
            background: linear-gradient(135deg, #00e676, #00ff88);
            box-shadow: 0 15px 40px rgba(0,255,136,0.6);
            transform: translateY(-3px);
        }

        .btn-clear {
            background: linear-gradient(135deg, #ff9800, #ffb74d);
            color: #000;
            font-weight: 700;
            grid-column: span 2;
        }

        .btn-clear:hover {
            background: linear-gradient(135deg, #f57c00, #ff9800);
            box-shadow: 0 10px 30px rgba(255,152,0,0.5);
        }

        .btn-function {
            background: linear-gradient(135deg, #9c27b0, #ce93d8);
            color: #fff;
            font-size: 1rem;
        }

        .btn-function:hover {
            background: linear-gradient(135deg, #8e24aa, #9c27b0);
            box-shadow: 0 10px 30px rgba(156,39,176,0.5);
        }

        /* Responsive */
        @media (max-width: 480px) {
            .calculator-container {
                padding: 1.5rem;
                margin: 1rem;
            }
            
            .display-input {
                font-size: 2rem;
            }
            
            .btn {
                height: 60px;
                font-size: 1.2rem;
            }
        }
    </style>
</head>
<body>
    <div class="calculator-container">
        <div class="header">
            <h1 class="title">DARUN CALC</h1>
            <p class="subtitle">Advanced Scientific Calculator</p>
        </div>

        <div class="display">
            <div class="display-output" id="previous">0</div>
            <div class="display-input" id="current">0</div>
        </div>

        <div class="buttons-grid">
            <button class="btn btn-function" onclick="clearAll()">AC</button>
            <button class="btn btn-function" onclick="deleteLast()">DEL</button>
            <button class="btn btn-function" onclick="toggleSign()">±</button>
            <button class="btn btn-function" onclick="calculatePercentage()">%</button>

            <button class="btn btn-number" onclick="inputDigit('7')">7</button>
            <button class="btn btn-number" onclick="inputDigit('8')">8</button>
            <button class="btn btn-number" onclick="inputDigit('9')">9</button>
            <button class="btn btn-operator" onclick="inputOperator('/')">÷</button>

            <button class="btn btn-number" onclick="inputDigit('4')">4</button>
            <button class="btn btn-number" onclick="inputDigit('5')">5</button>
            <button class="btn btn-number" onclick="inputDigit('6')">6</button>
            <button class="btn btn-operator" onclick="inputOperator('*')">×</button>

            <button class="btn btn-number" onclick="inputDigit('1')">1</button>
            <button class="btn btn-number" onclick="inputDigit('2')">2</button>
            <button class="btn btn-number" onclick="inputDigit('3')">3</button>
            <button class="btn btn-operator" onclick="inputOperator('-')">-</button>

            <button class="btn btn-number" onclick="inputDigit('0')" style="grid-column: span 2;">0</button>
            <button class="btn btn-number" onclick="inputDigit('.')">.</button>
            <button class="btn btn-equals" onclick="calculateResult()" style="grid-column: span 2;">=</button>
        </div>
    </div>

    <script>
        class Calculator {
            constructor() {
                this.current = '0';
                this.previous = '';
                this.operator = null;
                this.waitingForOperand = false;
                
                this.updateDisplay();
                this.bindKeyboard();
            }

            inputDigit(digit) {
                if (this.waitingForOperand) {
                    this.current = digit;
                    this.waitingForOperand = false;
                } else {
                    this.current = this.current === '0' ? digit : this.current + digit;
                }
                this.updateDisplay();
            }

            inputOperator(nextOperator) {
                const inputValue = parseFloat(this.current);

                if (this.operator) {
                    this.performCalculation(inputValue);
                } else {
                    this.previous = inputValue;
                }

                this.operator = nextOperator;
                this.waitingForOperand = true;
                this.updateDisplay();
            }

            performCalculation(secondOperand) {
                let result = 0;
                
                switch (this.operator) {
                    case '+':
                        result = parseFloat(this.previous) + secondOperand;
                        break;
                    case '-':
                        result = parseFloat(this.previous) - secondOperand;
                        break;
                    case '*':
                        result = parseFloat(this.previous) * secondOperand;
                        break;
                    case '/':
                        result = secondOperand === 0 ? 'Error' : parseFloat(this.previous) / secondOperand;
                        break;
                    default:
                        return;
                }

                this.current = this.formatResult(result);
                this.previous = this.current;
            }

            calculateResult() {
                if (this.operator) {
                    this.performCalculation(parseFloat(this.current));
                    this.operator = null;
                    this.waitingForOperand = true;
                }
                this.updateDisplay();
            }

            clearAll() {
                this.current = '0';
                this.previous = '';
                this.operator = null;
                this.waitingForOperand = false;
                this.updateDisplay();
            }

            deleteLast() {
                if (this.current.length > 1) {
                    this.current = this.current.slice(0, -1);
                } else {
                    this.current = '0';
                }
                this.updateDisplay();
            }

            toggleSign() {
                if (this.current !== '0' && this.current !== 'Error') {
                    this.current = this.current.startsWith('-') 
                        ? this.current.slice(1) 
                        : '-' + this.current;
                    this.updateDisplay();
                }
            }

            calculatePercentage() {
                const value = parseFloat(this.current);
                if (!isNaN(value)) {
                    this.current = (value / 100).toString();
                    this.updateDisplay();
                }
            }

            formatResult(num) {
                if (num === 'Error') return 'Error';
                
                if (Math.abs(num) > 1e12) {
                    return num.toExponential(6);
                } else if (Math.abs(num) > 999999999) {
                    return (num / 1e9).toFixed(2) + 'B';
                } else if (Math.abs(num) > 999999) {
                    return (num / 1e6).toFixed(2) + 'M';
                } else if (Math.abs(num) > 999) {
                    return (num / 1e3).toFixed(1) + 'K';
                }
                
                return parseFloat(num.toPrecision(12)).toString();
            }

            updateDisplay() {
                document.getElementById('current').textContent = this.current;
                document.getElementById('previous').textContent = 
                    this.previous && this.operator 
                        ? `${this.previous} ${this.operator}` 
                        : '';
            }

            bindKeyboard() {
                document.addEventListener('keydown', (event) => {
                    const key = event.key;
                    
                    if (key >= '0' && key <= '9') {
                        event.preventDefault();
                        this.inputDigit(key);
                    } else if (key === '.') {
                        event.preventDefault();
                        this.inputDigit('.');
                    } else if (['+', '-', '*', '/'].includes(key)) {
                        event.preventDefault();
                        this.inputOperator(key === '*' ? '*' : key === '/' ? '/' : key);
                    } else if (key === 'Enter' || key === '=') {
                        event.preventDefault();
                        this.calculateResult();
                    } else if (key === 'Escape' || key === 'c' || key === 'C') {
                        event.preventDefault();
                        this.clearAll();
                    } else if (key === 'Backspace') {
                        event.preventDefault();
                        this.deleteLast();
                    }
                });
            }
        }

        // Initialize calculator when page loads
        const calculator = new Calculator();
    </script>
</body>
</html># projects
