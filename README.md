
<html lang="ar" dir="rtl">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Switching Between Systems - Number Base Converter</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 10px;
            transition: all 0.3s ease;
        }
        
        body.dark-mode {
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
        }
        
        body.dark-mode .container {
            background: #2d3748;
            color: white;
        }
        
        body.dark-mode .input-group label {
            color: #e2e8f0;
        }
        
        body.dark-mode input[type="text"] {
            background: #4a5568;
            color: white;
            border-color: #718096;
        }
        
        body.dark-mode input[type="text"]:focus {
            background: #4a5568;
            border-color: #4facfe;
        }
        
        body.dark-mode input[type="text"]::placeholder {
            color: #a0aec0;
        }
        
        .container {
            max-width: 1200px;
            width: 100%;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            display: flex;
            flex-direction: column;
        }
        
        .header {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            text-align: center;
            padding: 20px;
            position: relative;
            min-height: 120px;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
        
        .header-controls {
            position: absolute;
            top: 15px;
            left: 15px;
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
        }
        
        .control-btn {
            background: rgba(255, 255, 255, 0.2);
            border: 2px solid rgba(255, 255, 255, 0.3);
            color: white;
            padding: 6px 12px;
            border-radius: 20px;
            cursor: pointer;
            font-size: 0.8em;
            font-weight: bold;
            transition: all 0.3s ease;
            backdrop-filter: blur(10px);
            white-space: nowrap;
        }
        
        .control-btn:hover {
            background: rgba(255, 255, 255, 0.3);
            transform: scale(1.05);
        }
        
        .base-selector {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(60px, 1fr));
            gap: 8px;
            margin: 15px 0;
            padding: 0 20px;
        }
        
        .base-btn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 15px;
            cursor: pointer;
            font-size: 0.85em;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            white-space: nowrap;
        }
        
        .base-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
        }
        
        .base-btn.active {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            box-shadow: 0 0 15px rgba(79, 172, 254, 0.4);
        }
        
        .header h1 {
            font-size: clamp(1.5rem, 4vw, 2.5rem);
            margin-bottom: 8px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        
        .header p {
            font-size: clamp(0.9rem, 2.5vw, 1.1rem);
            opacity: 0.9;
            margin: 2px 0;
        }
        
        .converter-section {
            padding: 20px;
            flex: 1;
            display: flex;
            flex-direction: column;
        }
        
        .input-group {
            margin-bottom: 20px;
        }
        
        .input-group label {
            display: block;
            font-size: clamp(1rem, 2.5vw, 1.1rem);
            font-weight: bold;
            margin-bottom: 10px;
            color: #333;
        }
        
        .input-container {
            position: relative;
            margin-bottom: 15px;
        }
        
        .base-label {
            position: absolute;
            top: -8px;
            right: 15px;
            background: #4facfe;
            color: white;
            padding: 4px 8px;
            border-radius: 10px;
            font-size: 0.7em;
            font-weight: bold;
            z-index: 1;
        }
        
        input[type="text"] {
            width: 100%;
            padding: 12px 16px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: clamp(0.9rem, 2.5vw, 1.1rem);
            transition: all 0.3s ease;
            background: #f8f9fa;
        }
        
        input[type="text"]::placeholder {
            color: #aaa;
            opacity: 0.7;
        }
        
        input[type="text"]:focus {
            outline: none;
            border-color: #4facfe;
            background: white;
            box-shadow: 0 0 0 3px rgba(79, 172, 254, 0.1);
        }
        
        .results-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 15px;
            margin-top: 20px;
            flex: 1;
        }
        
        .result-card {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            border-radius: 15px;
            padding: 15px;
            color: white;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
            transform: translateY(0);
            transition: transform 0.3s ease;
            position: relative;
            display: flex;
            flex-direction: column;
            min-height: 120px;
        }
        
        .result-card:hover {
            transform: translateY(-3px);
        }
        
        .result-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
            flex-wrap: wrap;
            gap: 8px;
        }
        
        .copy-btn {
            background: rgba(255, 255, 255, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.3);
            color: white;
            padding: 4px 8px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 0.75em;
            transition: all 0.3s ease;
            backdrop-filter: blur(5px);
            white-space: nowrap;
        }
        
        .copy-btn:hover {
            background: rgba(255, 255, 255, 0.3);
            transform: scale(1.05);
        }
        
        .copy-btn.copied {
            background: rgba(76, 175, 80, 0.8);
            border-color: rgba(76, 175, 80, 0.9);
        }
        
        .result-card h3 {
            font-size: clamp(0.9rem, 2.5vw, 1.2rem);
            margin: 0;
            flex: 1;
        }
        
        .result-value {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            padding: 12px;
            font-family: 'Courier New', monospace;
            font-size: clamp(0.8rem, 2vw, 1rem);
            text-align: center;
            word-break: break-all;
            flex: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 40px;
        }
        
        .clear-btn {
            background: linear-gradient(135deg, #ff6b6b, #ee5a24);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 25px;
            font-size: clamp(0.9rem, 2.5vw, 1rem);
            cursor: pointer;
            transition: all 0.3s ease;
            margin-top: 15px;
            align-self: center;
        }
        
        .clear-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }
        
        .error {
            background: #ff4757;
            color: white;
            padding: 8px 12px;
            border-radius: 10px;
            margin-top: 10px;
            font-size: clamp(0.8rem, 2vw, 0.9rem);
        }
        /* تحسينات للشاشات الصغيرة جداً */
        
        @media (max-width: 480px) {
            body {
                padding: 5px;
            }
            .container {
                border-radius: 15px;
            }
            .header {
                padding: 15px 10px;
                min-height: 100px;
            }
            .header-controls {
                position: static;
                justify-content: center;
                margin-bottom: 10px;
                gap: 5px;
            }
            .control-btn {
                padding: 4px 8px;
                font-size: 0.7em;
            }
            .base-selector {
                grid-template-columns: repeat(5, 1fr);
                gap: 5px;
                padding: 0 10px;
            }
            .base-btn {
                padding: 6px 4px;
                font-size: 0.7em;
            }
            .converter-section {
                padding: 15px 10px;
            }
            .results-grid {
                grid-template-columns: 1fr;
                gap: 10px;
            }
            .result-card {
                padding: 12px;
                min-height: 100px;
            }
            .result-header {
                flex-direction: column;
                align-items: flex-start;
                gap: 5px;
            }
            .copy-btn {
                align-self: flex-end;
            }
        }
        /* تحسينات للشاشات المتوسطة */
        
        @media (min-width: 481px) and (max-width: 768px) {
            .results-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            .base-selector {
                grid-template-columns: repeat(5, 1fr);
            }
        }
        /* تحسينات للشاشات الكبيرة */
        
        @media (min-width: 769px) and (max-width: 1024px) {
            .results-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
        /* تحسينات للشاشات الكبيرة جداً */
        
        @media (min-width: 1025px) {
            .results-grid {
                grid-template-columns: repeat(4, 1fr);
            }
            .converter-section {
                padding: 30px;
            }
            .header {
                padding: 25px;
            }
        }
        /* تحسينات للشاشات العريضة جداً */
        
        @media (min-width: 1400px) {
            .container {
                max-width: 1400px;
            }
        }
        /* تحسينات للوضع الأفقي للهواتف */
        
        @media (max-height: 500px) and (orientation: landscape) {
            .header {
                padding: 10px;
                min-height: 80px;
            }
            .header-controls {
                position: static;
                justify-content: center;
                margin-bottom: 5px;
            }
            .base-selector {
                margin: 10px 0;
            }
            .converter-section {
                padding: 15px;
            }
            .results-grid {
                grid-template-columns: repeat(4, 1fr);
            }
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="header">
            <div class="header-controls">
                <button class="control-btn" onclick="toggleLanguage()" id="lang-btn">🌐 English</button>
                <button class="control-btn" onclick="toggleDarkMode()" id="dark-btn">🌙 Dark</button>
            </div>
            <h1 id="main-title">Switching Between Systems</h1>
            <p id="subtitle">Number Base Converter</p>
            <p id="description">تحويل بين الأنظمة: عشري، ثنائي، ثماني، سادس عشر</p>

            <div class="base-selector">
                <button class="base-btn active" onclick="setInputBase('auto')" id="auto-btn">AUTO</button>
                <button class="base-btn" onclick="setInputBase('decimal')" id="dec-btn">DEC</button>
                <button class="base-btn" onclick="setInputBase('binary')" id="bin-btn">BIN</button>
                <button class="base-btn" onclick="setInputBase('octal')" id="oct-btn">OCT</button>
                <button class="base-btn" onclick="setInputBase('hexadecimal')" id="hex-btn">HEX</button>
            </div>
        </div>

        <div class="converter-section">
            <div class="input-group">
                <label id="input-label">أدخل الرقم في أي نظام:</label>
                <div class="input-container">
                    <div class="base-label" id="base-indicator">AUTO</div>
                    <input type="text" id="inputNumber" placeholder="مثال: 255 أو FF أو 11111111 أو 377">
                </div>
                <div id="error-message"></div>
                <button class="clear-btn" onclick="clearAll()" id="clear-btn">مسح الكل</button>
            </div>

            <div class="results-grid">
                <div class="result-card" style="background: linear-gradient(135deg, #36d1dc 0%, #5b86e5 100%);">
                    <div class="result-header">
                        <h3 id="decimal-title">عشري (Decimal)</h3>
                        <button class="copy-btn" onclick="copyResult('decimal')" id="copy-dec">📋 نسخ</button>
                    </div>
                    <div class="result-value" id="decimal-result">0</div>
                </div>

                <div class="result-card" style="background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%);">
                    <div class="result-header">
                        <h3 id="binary-title">ثنائي (Binary)</h3>
                        <button class="copy-btn" onclick="copyResult('binary')" id="copy-bin">📋 نسخ</button>
                    </div>
                    <div class="result-value" id="binary-result">0</div>
                </div>

                <div class="result-card" style="background: linear-gradient(135deg, #fa709a 0%, #fee140 100%);">
                    <div class="result-header">
                        <h3 id="octal-title">ثماني (Octal)</h3>
                        <button class="copy-btn" onclick="copyResult('octal')" id="copy-oct">📋 نسخ</button>
                    </div>
                    <div class="result-value" id="octal-result">0</div>
                </div>

                <div class="result-card" style="background: linear-gradient(135deg, #a8edea 0%, #fed6e3 100%); color: #333;">
                    <div class="result-header">
                        <h3 id="hex-title" style="color: #333;">سادس عشر (Hexadecimal)</h3>
                        <button class="copy-btn" onclick="copyResult('hex')" id="copy-hex" style="color: #333; border-color: rgba(51,51,51,0.3); background: rgba(51,51,51,0.1);">📋 نسخ</button>
                    </div>
                    <div class="result-value" id="hex-result" style="color: #333;">0</div>
                </div>
            </div>
        </div>
    </div>

    <script>
        const inputNumber = document.getElementById('inputNumber');
        const errorMessage = document.getElementById('error-message');
        const decimalResult = document.getElementById('decimal-result');
        const binaryResult = document.getElementById('binary-result');
        const octalResult = document.getElementById('octal-result');
        const hexResult = document.getElementById('hex-result');

        // Language variables
        let isArabic = true;
        let isDarkMode = false;
        let currentInputBase = 'auto';

        const translations = {
            arabic: {
                mainTitle: 'Switching Between Systems',
                subtitle: 'محول الأرقام',
                description: 'تحويل بين الأنظمة: عشري، ثنائي، ثماني، سادس عشر',
                inputLabel: 'أدخل الرقم في أي نظام:',
                placeholder: 'مثال: 255 أو FF أو 11111111 أو 377',
                clearBtn: 'مسح الكل',
                decimal: 'عشري (Decimal)',
                binary: 'ثنائي (Binary)',
                octal: 'ثماني (Octal)',
                hex: 'سادس عشر (Hexadecimal)',
                invalidNumber: 'رقم غير صالح! يرجى إدخال رقم صحيح.',
                conversionError: 'خطأ في التحويل!',
                langBtn: '🌐 English',
                darkBtn: '🌙 Dark',
                lightBtn: '☀️ Light',
                copyBtn: '📋 نسخ',
                copiedBtn: '✅ تم'
            },
            english: {
                mainTitle: 'Switching Between Systems',
                subtitle: 'Number Base Converter',
                description: 'Convert between: Decimal, Binary, Octal, Hexadecimal',
                inputLabel: 'Enter number in any base:',
                placeholder: 'Example: 255 or FF or 11111111 or 377',
                clearBtn: 'Clear All',
                decimal: 'Decimal (عشري)',
                binary: 'Binary (ثنائي)',
                octal: 'Octal (ثماني)',
                hex: 'Hexadecimal (سادس عشر)',
                invalidNumber: 'Invalid number! Please enter a valid number.',
                conversionError: 'Conversion error!',
                langBtn: '🌐 العربية',
                darkBtn: '🌙 Dark',
                lightBtn: '☀️ Light',
                copyBtn: '📋 Copy',
                copiedBtn: '✅ Done'
            }
        };

        function toggleLanguage() {
            isArabic = !isArabic;
            updateLanguage();
        }

        function toggleDarkMode() {
            isDarkMode = !isDarkMode;
            document.body.classList.toggle('dark-mode');
            updateLanguage();
        }

        function updateLanguage() {
            const lang = isArabic ? 'arabic' : 'english';
            const html = document.documentElement;

            // Update text direction
            if (isArabic) {
                html.setAttribute('dir', 'rtl');
                html.setAttribute('lang', 'ar');
            } else {
                html.setAttribute('dir', 'ltr');
                html.setAttribute('lang', 'en');
            }

            // Update all text elements
            document.getElementById('main-title').textContent = translations[lang].mainTitle;
            document.getElementById('subtitle').textContent = translations[lang].subtitle;
            document.getElementById('description').textContent = translations[lang].description;
            document.getElementById('input-label').textContent = translations[lang].inputLabel;
            document.getElementById('inputNumber').placeholder = translations[lang].placeholder;
            document.getElementById('clear-btn').textContent = translations[lang].clearBtn;
            document.getElementById('decimal-title').textContent = translations[lang].decimal;
            document.getElementById('binary-title').textContent = translations[lang].binary;
            document.getElementById('octal-title').textContent = translations[lang].octal;
            document.getElementById('hex-title').textContent = translations[lang].hex;
            document.getElementById('lang-btn').textContent = translations[lang].langBtn;
            document.getElementById('dark-btn').textContent = isDarkMode ? translations[lang].lightBtn : translations[lang].darkBtn;

            // Update copy buttons
            document.getElementById('copy-dec').textContent = translations[lang].copyBtn;
            document.getElementById('copy-bin').textContent = translations[lang].copyBtn;
            document.getElementById('copy-oct').textContent = translations[lang].copyBtn;
            document.getElementById('copy-hex').textContent = translations[lang].copyBtn;
        }

        function setInputBase(base) {
            currentInputBase = base;

            // Update active button
            document.querySelectorAll('.base-btn').forEach(btn => btn.classList.remove('active'));

            if (base === 'auto') {
                document.getElementById('auto-btn').classList.add('active');
                document.getElementById('base-indicator').textContent = 'AUTO';
            } else {
                const btnIds = {
                    decimal: 'dec-btn',
                    binary: 'bin-btn',
                    octal: 'oct-btn',
                    hexadecimal: 'hex-btn'
                };
                document.getElementById(btnIds[base]).classList.add('active');

                const baseNames = {
                    decimal: 'DEC',
                    binary: 'BIN',
                    octal: 'OCT',
                    hexadecimal: 'HEX'
                };
                document.getElementById('base-indicator').textContent = baseNames[base];
            }

            // Update placeholder based on mode
            if (base === 'auto') {
                document.getElementById('inputNumber').placeholder = isArabic ?
                    'مثال: 255 أو FF أو 11111111 أو 377' :
                    'Example: 255 or FF or 11111111 or 377';
            } else {
                const placeholders = {
                    decimal: isArabic ? 'مثال: 255' : 'Example: 255',
                    binary: isArabic ? 'مثال: 11111111' : 'Example: 11111111',
                    octal: isArabic ? 'مثال: 377' : 'Example: 377',
                    hexadecimal: isArabic ? 'مثال: FF' : 'Example: FF'
                };
                document.getElementById('inputNumber').placeholder = placeholders[base];
            }

            // Re-convert if there's input
            if (inputNumber.value.trim()) {
                convertNumber();
            }
        }

        function copyResult(type) {
            const results = {
                decimal: document.getElementById('decimal-result').textContent,
                binary: document.getElementById('binary-result').textContent,
                octal: document.getElementById('octal-result').textContent,
                hex: document.getElementById('hex-result').textContent
            };

            const copyButtons = {
                decimal: document.getElementById('copy-dec'),
                binary: document.getElementById('copy-bin'),
                octal: document.getElementById('copy-oct'),
                hex: document.getElementById('copy-hex')
            };

            navigator.clipboard.writeText(results[type]).then(() => {
                const btn = copyButtons[type];
                const lang = isArabic ? 'arabic' : 'english';
                const originalText = btn.textContent;

                btn.textContent = translations[lang].copiedBtn;
                btn.classList.add('copied');

                setTimeout(() => {
                    btn.textContent = originalText;
                    btn.classList.remove('copied');
                }, 2000);
            });
        }

        function clearError() {
            errorMessage.innerHTML = '';
        }

        function showError(message) {
            const lang = isArabic ? 'arabic' : 'english';
            errorMessage.innerHTML = `<div class="error">${translations[lang].invalidNumber}</div>`;
        }

        function detectBase(input) {
            input = input.trim().toUpperCase();

            // If user selected a specific base, validate for that base
            if (currentInputBase !== 'auto') {
                const patterns = {
                    decimal: /^[0-9]+$/,
                    binary: /^[01]+$/,
                    octal: /^[0-7]+$/,
                    hexadecimal: /^[0-9A-F]+$/
                };

                const bases = {
                    decimal: 10,
                    binary: 2,
                    octal: 8,
                    hexadecimal: 16
                };

                if (patterns[currentInputBase].test(input)) {
                    return {
                        base: bases[currentInputBase],
                        value: input
                    };
                } else {
                    return null;
                }
            }

            // Auto-detect (original logic)
            if (input.startsWith('0X')) {
                return {
                    base: 16,
                    value: input.substring(2)
                };
            }

            if (input.startsWith('0B')) {
                return {
                    base: 2,
                    value: input.substring(2)
                };
            }

            if (input.startsWith('0O')) {
                return {
                    base: 8,
                    value: input.substring(2)
                };
            }

            if (/^[01]+$/.test(input)) {
                return {
                    base: 2,
                    value: input
                };
            }

            if (/^[0-7]+$/.test(input)) {
                return {
                    base: 8,
                    value: input
                };
            }

            if (/^[0-9A-F]+$/.test(input)) {
                if (/[A-F]/.test(input)) {
                    return {
                        base: 16,
                        value: input
                    };
                } else {
                    return {
                        base: 10,
                        value: input
                    };
                }
            }

            if (/^[0-9]+$/.test(input)) {
                return {
                    base: 10,
                    value: input
                };
            }

            return null;
        }

        function convertNumber() {
            clearError();

            const input = inputNumber.value.trim();
            if (!input) {
                clearResults();
                return;
            }

            const detected = detectBase(input);
            if (!detected) {
                showError('Invalid number!');
                clearResults();
                return;
            }

            try {
                const decimalValue = parseInt(detected.value, detected.base);

                if (isNaN(decimalValue) || decimalValue < 0) {
                    showError('Invalid number!');
                    clearResults();
                    return;
                }

                // Update results
                decimalResult.textContent = decimalValue.toString();
                binaryResult.textContent = decimalValue.toString(2);
                octalResult.textContent = decimalValue.toString(8);
                hexResult.textContent = decimalValue.toString(16).toUpperCase();

            } catch (error) {
                showError('Conversion error!');
                clearResults();
            }
        }

        function clearResults() {
            decimalResult.textContent = '0';
            binaryResult.textContent = '0';
            octalResult.textContent = '0';
            hexResult.textContent = '0';
        }

        function clearAll() {
            inputNumber.value = '';
            clearError();
            clearResults();
        }

        inputNumber.addEventListener('input', convertNumber);
        inputNumber.addEventListener('keyup', convertNumber);

        // Initialize with 0
        clearResults();
    </script>
</body>

</html>
