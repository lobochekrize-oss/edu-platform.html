<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Образовательная платформа</title>
  <script src="https://unpkg.com/lucide@latest"></script>
  <style>
    :root {
      --bg-light: #fffaf0;
      --text-light: #1e293b;
      --card-light: #ffffff;
      --border-light: #f5e6d3;
      --button-light: #3b82f6;
      --button-hover-light: #2563eb;
      --secondary-light: #fef3c7;
      --input-light: #ffffff;
      --option-light: #fef3c7;
      --danger-light: #ef4444;
      --bg-dark: #0f172a;
      --text-dark: #f1f5f9;
      --card-dark: #1e293b;
      --border-dark: #334155;
      --button-dark: #3b82f6;
      --button-hover-dark: #2563eb;
      --secondary-dark: #334155;
      --input-dark: #0f172a;
      --option-dark: #334155;
      --danger-dark: #ef4444;
    }
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      transition: background-color 0.3s, color 0.3s;
      min-height: 100vh;
    }
    .bg-light { background-color: var(--bg-light); color: var(--text-light); }
    .bg-dark { background-color: var(--bg-dark); color: var(--text-dark); }
    .card-light { background-color: var(--card-light); border-color: var(--border-light); }
    .card-dark { background-color: var(--card-dark); border-color: var(--border-dark); }
    .button-light { background-color: var(--button-light); }
    .button-light:hover { background-color: var(--button-hover-light); }
    .button-dark { background-color: var(--button-dark); }
    .button-dark:hover { background-color: var(--button-hover-dark); }
    .secondary-light { background-color: var(--secondary-light); }
    .secondary-dark { background-color: var(--secondary-dark); }
    .input-light { background-color: var(--input-light); border-color: var(--border-light); color: var(--text-light); }
    .input-dark { background-color: var(--input-dark); border-color: var(--border-dark); color: var(--text-dark); }
    .option-light { background-color: var(--option-light); }
    .option-light:hover { background-color: #fde68a; }
    .option-dark { background-color: var(--option-dark); }
    .option-dark:hover { background-color: #475569; }
    .danger-light { background-color: var(--danger-light); }
    .danger-light:hover { background-color: #dc2626; }
    .danger-dark { background-color: var(--danger-dark); }
    .danger-dark:hover { background-color: #dc2626; }
    .hidden { display: none; }
  </style>
</head>
<body class="bg-light">
  <div id="root"></div>
  <script>
    // ===========================
    // ПОЛНОСТЬЮ РАБОЧИЙ КОД ПРИЛОЖЕНИЯ
    // ===========================
    const { useState, useEffect } = React;
    const { 
      Sun, Moon, BookOpen, User, GraduationCap, Plus,
      CheckCircle, Clock, Trophy, LogOut, ArrowLeft,
      BarChart2, List, FileText, Eye, Cloud, Upload, Database,
      X, Edit, Trash2, AlertTriangle
    } = lucideReact;

    const App = () => {
      const [currentUser, setCurrentUser] = useState(null);
      const [darkMode, setDarkMode] = useState(false);
      const [currentView, setCurrentView] = useState('auth');
      const [timeLeft, setTimeLeft] = useState(600);
      const [selectedAnswers, setSelectedAnswers] = useState({});
      const [currentTest, setCurrentTest] = useState(null);
      const [isRegistering, setIsRegistering] = useState(false);
      const [registerRole, setRegisterRole] = useState('student');
      const [cloudStatus, setCloudStatus] = useState('not_deployed');
      const [deploymentProgress, setDeploymentProgress] = useState(0);
      const [confirmDelete, setConfirmDelete] = useState(null);

      const [loginForm, setLoginForm] = useState({ email: '', password: '' });
      const [registerForm, setRegisterForm] = useState({
        name: '',
        email: '',
        password: '',
        confirmPassword: ''
      });

      const [teachers, setTeachers] = useState([
        { id: 't1', name: 'Анна Петрова', email: 'teacher@school.ru', password: 'teacher123' },
        { id: 't2', name: 'Иван Смирнов', email: 'ivan.math@school.ru', password: 'math456' }
      ]);
      const [students, setStudents] = useState([
        { id: 's1', name: 'Михаил Козлов', email: 'misha@student.ru', password: 'student123' },
        { id: 's2', name: 'Анна Иванова', email: 'anna@student.ru', password: 'anna456' }
      ]);
      const [tests, setTests] = useState([
        {
          id: 'test1',
          title: 'Основы Химии',
          teacherId: 't1',
          subject: 'Химия',
          questions: [
            {
              id: 1,
              question: "Какой элемент является основным в воде?",
              options: ["Кислород", "Водород", "Азот", "Углерод"],
              correctAnswer: "Водород"
            },
            {
              id: 2,
              question: "Какой элемент обозначается символом 'O'?",
              options: ["Золото", "Кислород", "Олово", "Осмий"],
              correctAnswer: "Кислород"
            }
          ]
        },
        {
          id: 'test2',
          title: 'Мировая История',
          teacherId: 't2',
          subject: 'История',
          questions: [
            {
              id: 1,
              question: "В каком году началась Вторая мировая война?",
              options: ["1914", "1939", "1941", "1945"],
              correctAnswer: "1939"
            },
            {
              id: 2,
              question: "Кто был первым президентом США?",
              options: ["Томас Джефферсон", "Джон Адамс", "Джордж Вашингтон", "Авраам Линкольн"],
              correctAnswer: "Джордж Вашингтон"
            }
          ]
        }
      ]);
      const [testResults, setTestResults] = useState([]);

      const bgClass = darkMode ? 'bg-dark' : 'bg-light';
      const textClass = darkMode ? 'text-dark' : 'text-light';
      const cardClass = darkMode ? 'card-dark' : 'card-light';
      const buttonClass = darkMode ? 'button-dark' : 'button-light';
      const secondaryButtonClass = darkMode ? 'secondary-dark' : 'secondary-light';
      const inputClass = darkMode ? 'input-dark' : 'input-light';
      const optionClass = darkMode ? 'option-dark' : 'option-light';
      const dangerButtonClass = darkMode ? 'danger-dark' : 'danger-light';

      const handleLoginChange = (e) => {
        const { name, value } = e.target;
        setLoginForm(prev => ({ ...prev, [name]: value }));
      };
      const handleRegisterChange = (e) => {
        const { name, value } = e.target;
        setRegisterForm(prev => ({ ...prev, [name]: value }));
      };

      const handleLogin = (e) => {
        e.preventDefault();
        const { email, password } = loginForm;

        const teacher = teachers.find(t => t.email === email && t.password === password);
        if (teacher) {
          setCurrentUser({ ...teacher, role: 'teacher' });
          setCurrentView('teacher-dashboard');
          setLoginForm({ email: '', password: '' });
          return;
        }

        const student = students.find(s => s.email === email && s.password === password);
        if (student) {
          setCurrentUser({ ...student, role: 'student' });
          setCurrentView('student-dashboard');
          setLoginForm({ email: '', password: '' });
          return;
        }

        alert('Неверный email или пароль');
      };

      const handleRegister = (e) => {
        e.preventDefault();
        const { name, email, password, confirmPassword } = registerForm;

        if (password !== confirmPassword) {
          alert('Пароли не совпадают');
          return;
        }
        if (password.length < 6) {
          alert('Пароль должен содержать не менее 6 символов');
          return;
        }

        if (registerRole === 'teacher') {
          if (teachers.some(t => t.email === email)) {
            alert('Учитель с таким email уже существует');
            return;
          }
          const newTeacher = { id: `t${Date.now()}`, name, email, password };
          setTeachers(prev => [...prev, newTeacher]);
          alert('Учитель успешно зарегистрирован!');
        } else {
          if (students.some(s => s.email === email)) {
            alert('Ученик с таким email уже существует');
            return;
          }
          const newStudent = { id: `s${Date.now()}`, name, email, password };
          setStudents(prev => [...prev, newStudent]);
          alert('Ученик успешно зарегистрирован!');
        }

        setIsRegistering(false);
        setRegisterForm({ name: '', email: '', password: '', confirmPassword: '' });
        setLoginForm({ email: '', password: '' });
      };

      const handleLogout = () => {
        setCurrentUser(null);
        setCurrentView('auth');
        setSelectedAnswers({});
        setCurrentTest(null);
        setLoginForm({ email: '', password: '' });
      };

      const startTest = (test) => {
        setCurrentTest(test);
        setSelectedAnswers({});
        setTimeLeft(600);
        setCurrentView('test-taking');
      };

      const handleAnswerSelect = (questionId, answer) => {
        setSelectedAnswers(prev => ({ ...prev, [questionId]: answer }));
      };

      const finishTest = () => {
        const alreadySubmitted = testResults.some(
          r => r.testId === currentTest.id && r.studentId === currentUser.id
        );

        if (alreadySubmitted) {
          alert('Вы уже прошли этот тест.');
          setCurrentView('student-dashboard');
          return;
        }

        let score = 0;
        currentTest.questions.forEach(question => {
          if (selectedAnswers[question.id] === question.correctAnswer) {
            score += Math.round(100 / currentTest.questions.length);
          }
        });

        const result = {
          id: Date.now(),
          testId: currentTest.id,
          studentId: currentUser.id,
          studentName: currentUser.name,
          score,
          date: new Date().toLocaleDateString('ru-RU'),
          answers: { ...selectedAnswers }
        };

        setTestResults(prev => [...prev, result]);
        setCurrentView('test-results');
      };

      const getTeacherResults = () => {
        return testResults.filter(result => {
          const test = tests.find(t => t.id === result.testId);
          return test && test.teacherId === currentUser.id;
        });
      };

      const handleDeleteTest = (testId) => {
        setTests(prev => prev.filter(test => test.id !== testId));
        setTestResults(prev => prev.filter(result => result.testId !== testId));
        setConfirmDelete(null);
        alert('Тест успешно удален');
      };

      const formatTime = (seconds) => {
        const mins = Math.floor(seconds / 60);
        const secs = seconds % 60;
        return `${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
      };

      useEffect(() => {
        let timer;
        if (currentView === 'test-taking' && timeLeft > 0) {
          timer = setInterval(() => {
            setTimeLeft(prev => prev - 1);
          }, 1000);
        } else if (timeLeft === 0) {
          finishTest();
        }
        return () => clearInterval(timer);
      }, [timeLeft, currentView]);

      const deployToCloud = () => {
        setCloudStatus('deploying');
        let progress = 0;
        const interval = setInterval(() => {
          progress += 5;
          setDeploymentProgress(progress);
          if (progress >= 100) {
            clearInterval(interval);
            setCloudStatus('deployed');
            setTimeout(() => setCloudStatus('active'), 1000);
          }
        }, 200);
      };

      const redeployToCloud = () => {
        setCloudStatus('deploying');
        setDeploymentProgress(0);
        setTimeout(() => {
          setDeploymentProgress(100);
          setCloudStatus('active');
        }, 2000);
      };

      const AuthView = () => (
        React.createElement("div", { className: "min-h-screen flex items-center justify-center p-4" },
          React.createElement("div", { className: `w-full max-w-md rounded-2xl border-2 p-8 ${cardClass} shadow-lg` },
            React.createElement("div", { className: "text-center mb-8" },
              React.createElement("div", { className: "flex justify-center mb-4" },
                React.createElement(BookOpen, { className: `w-12 h-12 ${darkMode ? 'text-blue-400' : 'text-blue-500'}` })
              ),
              React.createElement("h1", { className: "text-
