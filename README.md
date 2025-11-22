# Chess
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>شطرنجي - تعلم الشطرنج من الصفر</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            color: white;
            min-height: 100vh;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        /* شريط التنقل */
        nav {
            background: rgba(0, 0, 0, 0.8);
            padding: 1rem;
            border-radius: 10px;
            margin-bottom: 20px;
        }

        .nav-links {
            display: flex;
            justify-content: space-around;
            list-style: none;
        }

        .nav-links a {
            color: white;
            text-decoration: none;
            padding: 10px 20px;
            border-radius: 5px;
            transition: background 0.3s;
        }

        .nav-links a:hover {
            background: #4a90e2;
        }

        /* الشبكة الرئيسية */
        .main-grid {
            display: grid;
            grid-template-columns: 1fr 400px;
            gap: 20px;
            margin-bottom: 20px;
        }

        /* لوحة الشطرنج */
        .chess-board {
            background: #fff;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
        }

        .board {
            width: 100%;
            max-width: 500px;
            margin: 0 auto;
            border: 10px solid #8B4513;
        }

        .row {
            display: flex;
        }

        .square {
            width: 60px;
            height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            cursor: pointer;
            transition: background 0.3s;
            position: relative;
        }

        .white {
            background: #f0d9b5;
        }

        .black {
            background: #b58863;
        }

        .square:hover {
            background: #ffeb3b !important;
        }

        .selected {
            background: #aaffaa !important;
        }

        .valid-move {
            position: relative;
        }

        .valid-move::after {
            content: "";
            position: absolute;
            width: 20px;
            height: 20px;
            background: rgba(0, 255, 0, 0.3);
            border-radius: 50%;
        }

        /* اللوحة الجانبية */
        .side-panel {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 20px;
            backdrop-filter: blur(10px);
        }

        .progress-section {
            background: rgba(255, 255, 255, 0.2);
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 20px;
        }

        .progress-bar {
            background: #333;
            height: 10px;
            border-radius: 5px;
            margin: 10px 0;
            overflow: hidden;
        }

        .progress {
            background: #4CAF50;
            height: 100%;
            transition: width 0.3s;
        }

        /* أقسام التعليم */
        .learning-sections {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }

        .section-card {
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            cursor: pointer;
            transition: transform 0.3s;
        }

        .section-card:hover {
            transform: translateY(-5px);
            background: rgba(255, 255, 255, 0.2);
        }

        .section-card h3 {
            margin-bottom: 10px;
            color: #4a90e2;
        }

        /* منطقة الفيديو */
        .video-section {
            background: rgba(0, 0, 0, 0.7);
            border-radius: 10px;
            padding: 20px;
            margin-top: 20px;
            display: none;
        }

        .video-container {
            position: relative;
            padding-bottom: 56.25%;
            height: 0;
            margin: 20px 0;
        }

        .video-container iframe {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            border-radius: 10px;
        }

        /* الأزرار */
        .btn {
            background: #4a90e2;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1rem;
            margin: 5px;
            transition: background 0.3s;
        }

        .btn:hover {
            background: #357abd;
        }

        .btn-success {
            background: #4CAF50;
        }

        .btn-success:hover {
            background: #45a049;
        }

        .btn-danger {
            background: #f44336;
        }

        .btn-danger:hover {
            background: #da190b;
        }

        /* تحكم الكمبيوتر */
        .computer-controls {
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            margin-top: 15px;
        }

        .difficulty-select {
            width: 100%;
            padding: 8px;
            margin: 10px 0;
            border-radius: 5px;
            border: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- شريط التنقل -->
        <nav>
            <ul class="nav-links">
                <li><a href="#home" onclick="showSection('home')">الرئيسية</a></li>
                <li><a href="#learn" onclick="showSection('learn')">التعلم</a></li>
                <li><a href="#practice" onclick="showSection('practice')">التدريب</a></li>
                <li><a href="#play" onclick="showSection('play')">اللعب</a></li>
                <li><a href="#progress" onclick="showSection('progress')">التقدم</a></li>
                <li><a href="#videos" onclick="showSection('videos')">دروس الفيديو</a></li>
            </ul>
        </nav>

        <!-- الشبكة الرئيسية -->
        <div class="main-grid">
            <!-- لوحة الشطرنج -->
            <div class="chess-board">
                <h2 style="text-align: center; margin-bottom: 20px; color: #333;" id="boardTitle">لوحة الشطرنج التفاعلية</h2>
                <div class="board" id="chessBoard">
                    <!-- سيتم إنشاء اللوحة بالجافاسكريبت -->
                </div>
                <div style="text-align: center; margin-top: 20px;">
                    <button class="btn" onclick="resetBoard()">إعادة الضبط</button>
                    <button class="btn btn-success" onclick="startTutorial()">بدء الدرس</button>
                    <button class="btn" onclick="playVsComputer()">العب ضد الكمبيوتر</button>
                </div>

                <!-- تحكم الكمبيوتر -->
                <div class="computer-controls" id="computerControls" style="display: none;">
                    <h4 style="color: #333; text-align: center;">العب ضد الكمبيوتر</h4>
                    <select class="difficulty-select" id="difficulty">
                        <option value="1">سهل (مبتدئ)</option>
                        <option value="2">متوسط</option>
                        <option value="3">صعب (متقدم)</option>
                        <option value="4">خبير</option>
                    </select>
                    <div style="text-align: center;">
                        <button class="btn btn-success" onclick="makeComputerMove()">اترك الكمبيوتر يلعب</button>
                        <button class="btn btn-danger" onclick="stopComputerGame()">توقف عن اللعب</button>
                    </div>
                </div>
            </div>

            <!-- اللوحة الجانبية -->
            <div class="side-panel">
                <h3>تقدمك في التعلم</h3>
                <div class="progress-section">
                    <p>إكمال الدروس الأساسية</p>
                    <div class="progress-bar">
                        <div class="progress" id="lessonsProgress"></div>
                    </div>
                    <span id="lessonsText">0% مكتمل</span>
                </div>

                <div class="progress-section">
                    <p>حل الألغاز</p>
                    <div class="progress-bar">
                        <div class="progress" id="puzzlesProgress"></div>
                    </div>
                    <span id="puzzlesText">0% مكتمل</span>
                </div>

                <div class="progress-section">
                    <p>مهارات اللعب</p>
                    <div class="progress-bar">
                        <div class="progress" id="skillsProgress"></div>
                    </div>
                    <span id="skillsText">0% مكتمل</span>
                </div>

                <h3 style="margin-top: 30px;" id="lessonTitle">الدرس الحالي</h3>
                <div id="currentLesson">
                    <p id="lessonDescription">اختر درساً للبدء...</p>
                    <button class="btn" onclick="nextLesson()" id="nextLessonBtn" style="display: none;">الدرس التالي</button>
                    <button class="btn" onclick="completeLesson()" id="completeLessonBtn" style="display: none;">أكمل الدرس</button>
                </div>
            </div>
        </div>

        <!-- قسم دروس الفيديو -->
        <div class="video-section" id="videoSection">
            <h3 style="text-align: center; margin-bottom: 20px;">دروس الفيديو التعليمية</h3>
            <div class="video-container">
                <iframe id="educationalVideo" width="560" height="315" src="" frameborder="0" 
                    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
                    allowfullscreen>
                </iframe>
            </div>
            <div style="text-align: center;">
                <button class="btn" onclick="loadVideo('basics')">أساسيات الشطرنج</button>
                <button class="btn" onclick="loadVideo('openings')">الافتتاحيات</button>
                <button class="btn" onclick="loadVideo('tactics')">التكتيكات</button>
                <button class="btn" onclick="loadVideo('endgames')">نهايات اللعبة</button>
            </div>
        </div>

        <!-- أقسام التعليم -->
        <div class="learning-sections" id="learningSections">
            <div class="section-card" onclick="startPieceTutorial()">
                <h3>🧩 تعلم القطع</h3>
                <p>تعرف على كل قطعة وطريقة حركتها</p>
            </div>
            <div class="section-card" onclick="startRulesTutorial()">
                <h3>📚 القواعد الأساسية</h3>
                <p>التبييت، الترقية، كش ملك</p>
            </div>
            <div class="section-card" onclick="startTacticsTutorial()">
                <h3>🎯 التكتيكات</h3>
                <p>الشوكة، التثبيت، الكش المزدوج</p>
            </div>
            <div class="section-card" onclick="showVideoSection()">
                <h3>🎥 دروس الفيديو</h3>
                <p>شروح مرئية مبسطة</p>
            </div>
        </div>
    </div>

    <!-- مكتبة Stockfish.js -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/stockfish.js/10.0.2/stockfish.js"></script>
    
    <script>
        // 🔧 إعدادات قاعدة البيانات
        const DB_KEYS = {
            USER_PROGRESS: 'chess_user_progress',
            COMPLETED_LESSONS: 'chess_completed_lessons',
            PUZZLES_SOLVED: 'chess_puzzles_solved',
            GAMES_PLAYED: 'chess_games_played'
        };

        // 💾 نظام قاعدة البيانات
        class ChessDatabase {
            static get(key) {
                const data = localStorage.getItem(key);
                return data ? JSON.parse(data) : null;
            }

            static set(key, value) {
                localStorage.setItem(key, JSON.stringify(value));
            }

            static initializeUserData() {
                if (!this.get(DB_KEYS.USER_PROGRESS)) {
                    this.set(DB_KEYS.USER_PROGRESS, {
                        lessonsCompleted: 0,
                        totalLessons: 10,
                        puzzlesSolved: 0,
                        totalPuzzles: 20,
                        skillsLevel: 0,
                        gamesPlayed: 0,
                        currentLesson: 0
                    });
                }

                if (!this.get(DB_KEYS.COMPLETED_LESSONS)) {
                    this.set(DB_KEYS.COMPLETED_LESSONS, []);
                }

                if (!this.get(DB_KEYS.PUZZLES_SOLVED)) {
                    this.set(DB_KEYS.PUZZLES_SOLVED, []);
                }
            }

            static updateProgress() {
                const progress = this.get(DB_KEYS.USER_PROGRESS);
                const completedLessons = this.get(DB_KEYS.COMPLETED_LESSONS).length;
                
                progress.lessonsCompleted = completedLessons;
                progress.puzzlesSolved = this.get(DB_KEYS.PUZZLES_SOLVED).length;
                progress.skillsLevel = Math.min(100, Math.floor((completedLessons / progress.totalLessons) * 100));
                
                this.set(DB_KEYS.USER_PROGRESS, progress);
                this.updateUI();
            }

            static updateUI() {
                const progress = this.get(DB_KEYS.USER_PROGRESS);
                
                // تحديث أشرطة التقدم
                document.getElementById('lessonsProgress').style.width = 
                    ${(progress.lessonsCompleted / progress.totalLessons) * 100}%;
                document.getElementById('puzzlesProgress').style.width = 
                    ${(progress.puzzlesSolved / progress.totalPuzzles) * 100}%;
                document.getElementById('skillsProgress').style.width = ${progress.skillsLevel}%;
                
                // تحديث النصوص
                document.getElementById('lessonsText').textContent = 
                    ${Math.round((progress.lessonsCompleted / progress.totalLessons) * 100)}% مكتمل;
                document.getElementById('puzzlesText').textContent = 
                    ${Math.round((progress.puzzlesSolved / progress.totalPuzzles) * 100)}% مكتمل;
                document.getElementById('skillsText').textContent = ${progress.skillsLevel}% مكتمل;
            }

            static completeLesson(lessonId) {
                const completed = this.get(DB_KEYS.COMPLETED_LESSONS);
                if (!completed.includes(lessonId)) {
                    completed.push(lessonId);
                    this.set(DB_KEYS.COMPLETED_LESSONS, completed);
                    this.updateProgress();
                }
            }
        }

        // ♟ محرك الشطرنج (Stockfish)
        class ChessEngine {
            constructor() {
                this.engine = Stockfish();
                this.isReady = false;
                this.setupEngine();
            }

            setupEngine() {
                this.engine.onmessage = (event) => {
                    const message = event.data || event;
                    console.log('Stockfish:', message);

                    if (message === 'uciok') {
                        this.isReady = true;
                        console.log('محرك الشطرنج جاهز!');
                    } else if (message.startsWith('bestmove')) {
                        const move = message.split(' ')[1];
                        if (move && move !== 'none') {
                            this.onBestMove(move);
                        }
                    }
                };

                // إعداد المحرك
                this.engine.postMessage('uci');
                this.engine.postMessage('isready');
                this.setDifficulty(2);
            }

            setDifficulty(level) {
                const levels = {
                    1: { skill: 5, depth: 10 },
                    2: { skill: 15, depth: 12 },
                    3: { skill: 20, depth: 15 },
                    4: { skill: 25, depth: 18 }
                };
                
                const config = levels[level] || levels[2];
                this.engine.postMessage(setoption name Skill Level value ${config.skill});
                this.engine.postMessage(setoption name Contempt value 0);
            }

            getBestMove(fen, time = 1000) {
                if (!this.isReady) {
                    console.log('المحرك ليس جاهزاً بعد');
                    return;
                }

                this.engine.postMessage(position fen ${fen});
                this.engine.postMessage(go movetime ${time});
            }

            onBestMove(move) {
                if (window.handleComputerMove) {
                    window.handleComputerMove(move);
                }
            }
        }

        // 🎮 نظام اللعبة الأساسي
        let currentBoard = [];
        let selectedPiece = null;
        let validMoves = [];
        let chessEngine = null;
        let isComputerGame = false;

        // رموز القطع Unicode
        const pieceSymbols = {
            'r': '♜', 'n': '♞', 'b': '♝', 'q': '♛', 'k': '♚', 'p': '♟',
            'R': '♖', 'N': '♘', 'B': '♗', 'Q': '♕', 'K': '♔', 'P': '♙'
        };

        // تهيئة اللوحة الابتدائية
        const initialBoard = [
            ['r', 'n', 'b', 'q', 'k', 'b', 'n', 'r'],
            ['p', 'p', 'p', 'p', 'p', 'p', 'p', 'p'],
            ['', '', '', '', '', '', '', ''],
            ['', '', '', '', '', '', '', ''],
            ['', '', '', '', '', '', '', ''],
            ['', '', '', '', '', '', '', ''],
            ['P', 'P', 'P', 'P', 'P', 'P', 'P', 'P'],
            ['R', 'N', 'B', 'Q', 'K', 'B', 'N', 'R']
        ];

        // إنشاء لوحة الشطرنج
        function createChessBoard() {
            const board = document.getElementById('chessBoard');
            board.innerHTML = '';
            currentBoard = JSON.parse(JSON.stringify(initialBoard));

            for (let row = 0; row < 8; row++) {
                const rowElement = document.createElement('div');
                rowElement.className = 'row';

                for (let col = 0; col < 8; col++) {
                    const square = document.createElement('div');
                    square.className = square ${(row + col) % 2 === 0 ? 'white' : 'black'};
                    square.dataset.row = row;
                    square.dataset.col = col;

                    const piece = currentBoard[row][col];
                    if (piece) {
                        square.textContent = pieceSymbols[piece];
                        square.style.cursor = 'grab';
                    }

                    square.addEventListener('click', () => handleSquareClick(row, col));
                    rowElement.appendChild(square);
                }
                board.appendChild(rowElement);
            }
        }

        // التعامل مع النقر على المربعات
        function handleSquareClick(row, col) {
            const piece = currentBoard[row][col];
            
            if (selectedPiece) {
                // محاولة تحريك القطعة
                if (isValidMove(row, col)) {
                    movePiece(selectedPiece.row, selectedPiece.col, row, col);
                    clearSelection();
                    
                    if (isComputerGame) {
                        // انتظر ثم دع الكمبيوتر يلعب
                        setTimeout(() => makeComputerMove(), 500);
                    }
                    return;
                }
                clearSelection();
            }

            if (piece && (isComputerGame ? piece === piece.toUpperCase() : true)) {
                selectedPiece = { row, col, piece };
                document.querySelector(.square[data-row="${row}"][data-col="${col}"]).classList.add('selected');
                showValidMoves(row, col, piece);
            }
        }

        // عرض الحركات المتاحة
        function showValidMoves(row, col, piece) {
            validMoves = getValidMoves(row, col, piece);
            validMoves.forEach(move => {
                const square = document.querySelector(.square[data-row="${move.row}"][data-col="${move.col}"]);
                if (square) {
                    square.classList.add('valid-move');
                }
            });
        }

        // الحصول على الحركات المتاحة (مبسطة)
        function getValidMoves(row, col, piece) {
            const moves = [];
            const isWhite = piece === piece.toUpperCase();

            // حركات البيدق المبسطة
            if (piece.toLowerCase() === 'p') {
                const direction = isWhite ? -1 : 1;
                const startRow = isWhite ? 6 : 1;

                // حركة أمامية
                if (currentBoard[row + direction] && !currentBoard[row + direction][col]) {
                    moves.push({ row: row + direction, col });
                    
                    // حركة مزدوجة من الموضع الابتدائي
                    if (row === startRow && !currentBoard[row + 2 * direction][col]) {
                        moves.push({ row: row + 2 * direction, col });
                    }
                }

                // الأكل بشكل مائل
                [-1, 1].forEach(offset => {
                    if (currentBoard[row + direction] && currentBoard[row + direction][col + offset]) {
                        const targetPiece = currentBoard[row + direction][col + offset];
                        if (targetPiece && (isWhite !== (targetPiece === targetPiece.toUpperCase()))) {
                            moves.push({ row: row + direction, col: col + offset });
                        }
                    }
                });
            }

            return moves;
        }

        // تحريك القطعة
        function movePiece(fromRow, fromCol, toRow, toCol) {
            currentBoard[toRow][toCol] = currentBoard[fromRow][fromCol];
            currentBoard[fromRow][fromCol] = '';
            updateBoardDisplay();
            
            // تحديث التقدم في قاعدة البيانات
            const progress = ChessDatabase.get(DB_KEYS.USER_PROGRESS);
            progress.gamesPlayed++;
            ChessDatabase.set(DB_KEYS.USER_PROGRESS, progress);
            ChessDatabase.updateUI();
        }

        // تحديث عرض اللوحة
        function updateBoardDisplay() {
            const squares = document.querySelectorAll('.square');
            squares.forEach(square => {
                const row = parseInt(square.dataset.row);
                const col = parseInt(square.dataset.col);
                const piece = currentBoard[row][col];
                
                square.textContent = piece ? pieceSymbols[piece] : '';
                square.classList.remove('selected', 'valid-move');
            });
        }

        // التحقق من صحة الحركة
        function isValidMove(row, col) {
            return validMoves.some(move => move.row === row && move.col === col);
        }

        // مسح التحديد
        function clearSelection() {
            document.querySelectorAll('.square').forEach(square => {
                square.classList.remove('selected', 'valid-move');
            });
            selectedPiece = null;
            validMoves = [];
        }

        // 🎯 نظام الدروس
        const lessons = [
            {
                id: 1,
                title: "حركة البيدق",
                description: "يتحرك البيدق مربعاً واحداً للأمام، وفي الحركة الأولى يمكنه التحرك مربعين",
                videoId: "basics"
            },
            {
                id: 2,
                title: "حركة الرخ",
                description: "الرخ يتحرك أفقياً أو عمودياً أي عدد من المربعات",
                videoId: "basics"
            },
            {
                id: 3,
                title: "حركة الفيل",
                description: "الفيل يتحرك بشكل مائل أي عدد من المربعات",
                videoId: "basics"
            },
            {
                id: 4,
                title: "كش ملك",
                description: "تعلم كيف تهدد ملك الخصم وتنتهي اللعبة",
                videoId: "tactics"
            }
        ];

        function startTutorial() {
            document.getElementById('lessonTitle').textContent = lessons[0].title;
            document.getElementById('lessonDescription').textContent = lessons[0].description;
            document.getElementById('nextLessonBtn').style.display = 'inline-block';
            document.getElementById('completeLessonBtn').style.display = 'inline-block';
        }

        function nextLesson() {
            const progress = ChessDatabase.get(DB_KEYS.USER_PROGRESS);
            const nextLessonIndex = (progress.currentLesson + 1) % lessons.length;
            const lesson = lessons[nextLessonIndex];
            
            document.getElementById('lessonTitle').textContent = lesson.title;
            document.getElementById('lessonDescription').textContent = lesson.description;
            
            progress.currentLesson = nextLessonIndex;
            ChessDatabase.set(DB_KEYS.USER_PROGRESS, progress);
        }

        function completeLesson() {
            const progress = ChessDatabase.get(DB_KEYS.USER_PROGRESS);
            ChessDatabase.completeLesson(progress.currentLesson);
            alert('مبروك! أكملت الدرس بنجاح 🎉');
        }

        // 🖥 اللعب ضد الكمبيوتر
        function playVsComputer() {
            isComputerGame = true;
            document.getElementById('computerControls').style.display = 'block';
            document.getElementById('boardTitle').textContent = 'العب ضد الكمبيوتر';
            createChessBoard();
            
            if (!chessEngine) {
                chessEngine = new ChessEngine();
            }
        }

        function makeComputerMove() {
            if (!chessEngine || !isComputerGame) return;

            // تحويل اللوحة الحالية إلى تنسيق FEN مبسط
            const fen = generateSimpleFEN();
            chessEngine.getBestMove(fen, 2000);
        }

        // معالجة حركة الكمبيوتر
        window.handleComputerMove = function(move) {
            if (!isComputerGame) return;

            // تحويل الحركة من تنسيق مثل "e2e4" إلى إحداثيات
            const fromCol = move.charCodeAt(0) - 97;
            const fromRow = 8 - parseInt(move.charAt(1));
            const toCol = move.charCodeAt(2) - 97;
            const toRow = 8 - parseInt(move.charAt(3));

            movePiece(fromRow, fromCol, toRow, toCol);
        };

        function stopComputerGame() {
            isComputerGame = false;
            document.getElementById('computerControls').style.display = 'none';
            document.getElementById('boardTitle').textContent = 'لوحة الشطرنج التفاعلية';
            createChessBoard();
        }

        // 🎥 نظام دروس الفيديو
        function showVideoSection() {
            document.getElementById('videoSection').style.display = 'block';
            document.getElementById('learningSections').style.display = 'none';
            loadVideo('basics');
        }

        function loadVideo(type) {
            const videoUrls = {
                'basics': 'https://www.youtube.com/embed/OCSbzArwB10',
                'openings': 'https://www.youtube.com/embed/6PlyskIKSIo',
                'tactics': 'https://www.youtube.com/embed/J7Fm0Jr
