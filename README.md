import React, { useState, useEffect, useRef } from 'react';
import { Code, Rocket, Cloud, Zap, Terminal, Github, Linkedin, Mail, Star, GitBranch, Users, Award } from 'lucide-react';

const GitHubProfile = () => {
  const [score, setScore] = useState(0);
  const [gameActive, setGameActive] = useState(false);
  const [snake, setSnake] = useState([[10, 10]]);
  const [food, setFood] = useState([15, 15]);
  const [direction, setDirection] = useState([0, 1]);
  const [particles, setParticles] = useState([]);
  const [mousePos, setMousePos] = useState({ x: 0, y: 0 });
  const gameRef = useRef(null);

  // Particle system for background
  useEffect(() => {
    const newParticles = Array.from({ length: 50 }, (_, i) => ({
      id: i,
      x: Math.random() * 100,
      y: Math.random() * 100,
      size: Math.random() * 3 + 1,
      speedX: (Math.random() - 0.5) * 0.5,
      speedY: (Math.random() - 0.5) * 0.5
    }));
    setParticles(newParticles);

    const interval = setInterval(() => {
      setParticles(prev => prev.map(p => ({
        ...p,
        x: (p.x + p.speedX + 100) % 100,
        y: (p.y + p.speedY + 100) % 100
      })));
    }, 50);

    return () => clearInterval(interval);
  }, []);

  // Snake game logic
  useEffect(() => {
    if (!gameActive) return;

    const handleKeyPress = (e) => {
      const keyMap = {
        ArrowUp: [0, -1],
        ArrowDown: [0, 1],
        ArrowLeft: [-1, 0],
        ArrowRight: [1, 0]
      };
      if (keyMap[e.key]) {
        const newDir = keyMap[e.key];
        if (newDir[0] !== -direction[0] || newDir[1] !== -direction[1]) {
          setDirection(newDir);
        }
      }
    };

    window.addEventListener('keydown', handleKeyPress);
    return () => window.removeEventListener('keydown', handleKeyPress);
  }, [direction, gameActive]);

  useEffect(() => {
    if (!gameActive) return;

    const gameLoop = setInterval(() => {
      setSnake(prev => {
        const head = prev[0];
        const newHead = [head[0] + direction[0], head[1] + direction[1]];

        // Check wall collision
        if (newHead[0] < 0 || newHead[0] >= 20 || newHead[1] < 0 || newHead[1] >= 20) {
          setGameActive(false);
          return prev;
        }

        // Check self collision
        if (prev.some(segment => segment[0] === newHead[0] && segment[1] === newHead[1])) {
          setGameActive(false);
          return prev;
        }

        const newSnake = [newHead, ...prev];

        // Check food collision
        if (newHead[0] === food[0] && newHead[1] === food[1]) {
          setScore(s => s + 10);
          setFood([Math.floor(Math.random() * 20), Math.floor(Math.random() * 20)]);
        } else {
          newSnake.pop();
        }

        return newSnake;
      });
    }, 150);

    return () => clearInterval(gameLoop);
  }, [direction, food, gameActive]);

  const startGame = () => {
    setSnake([[10, 10]]);
    setFood([15, 15]);
    setDirection([0, 1]);
    setScore(0);
    setGameActive(true);
  };

  const stats = [
    { icon: Star, label: 'Total Stars', value: '150+', color: 'from-yellow-400 to-orange-500' },
    { icon: GitBranch, label: 'Projects', value: '25+', color: 'from-green-400 to-emerald-500' },
    { icon: Users, label: 'Followers', value: '500+', color: 'from-blue-400 to-cyan-500' },
    { icon: Award, label: 'Contributions', value: '1.2K+', color: 'from-purple-400 to-pink-500' }
  ];

  const techStack = [
    { name: 'React', color: 'bg-cyan-500', level: 95 },
    { name: 'Node.js', color: 'bg-green-500', level: 90 },
    { name: 'TypeScript', color: 'bg-blue-500', level: 88 },
    { name: 'AWS', color: 'bg-orange-500', level: 85 },
    { name: 'Python', color: 'bg-yellow-500', level: 82 },
    { name: 'Docker', color: 'bg-sky-500', level: 80 }
  ];

  const projects = [
    {
      title: '🌌 AstroAtlas',
      desc: '3D Space Exploration with NASA APIs',
      tech: ['React', 'Three.js', 'WebGL'],
      gradient: 'from-indigo-500 to-purple-600'
    },
    {
      title: '🚨 Disaster AI',
      desc: 'ML-Powered Emergency Response',
      tech: ['FastAPI', 'TensorFlow', 'MongoDB'],
      gradient: 'from-red-500 to-pink-600'
    },
    {
      title: '🏥 MediSphere',
      desc: 'Smart Hospital Management Suite',
      tech: ['Next.js', 'OpenAI', 'AWS'],
      gradient: 'from-emerald-500 to-teal-600'
    }
  ];

  return (
    <div 
      className="min-h-screen bg-gradient-to-br from-gray-900 via-purple-900 to-gray-900 text-white overflow-hidden relative"
      onMouseMove={(e) => setMousePos({ x: e.clientX, y: e.clientY })}
    >
      {/* Animated Background Particles */}
      <div className="absolute inset-0 overflow-hidden pointer-events-none">
        {particles.map(p => (
          <div
            key={p.id}
            className="absolute rounded-full bg-purple-400 opacity-20"
            style={{
              left: `${p.x}%`,
              top: `${p.y}%`,
              width: `${p.size}px`,
              height: `${p.size}px`,
              filter: 'blur(1px)'
            }}
          />
        ))}
      </div>

      {/* Cursor Glow Effect */}
      <div
        className="absolute w-96 h-96 rounded-full pointer-events-none transition-all duration-300 ease-out"
        style={{
          left: mousePos.x - 192,
          top: mousePos.y - 192,
          background: 'radial-gradient(circle, rgba(139,92,246,0.15) 0%, transparent 70%)',
          filter: 'blur(40px)'
        }}
      />

      <div className="relative z-10 max-w-7xl mx-auto px-4 py-12">
        {/* Header with Animation */}
        <div className="text-center mb-16 animate-fade-in">
          <div className="inline-block mb-6">
            <div className="w-32 h-32 rounded-full bg-gradient-to-br from-purple-500 to-pink-500 mx-auto flex items-center justify-center text-6xl font-bold shadow-2xl animate-pulse-slow">
              SP
            </div>
          </div>
          <h1 className="text-6xl font-bold mb-4 bg-gradient-to-r from-purple-400 via-pink-400 to-cyan-400 bg-clip-text text-transparent animate-gradient">
            SAKETH PAGGILLA
          </h1>
          <div className="flex items-center justify-center gap-4 text-xl text-purple-300 mb-6">
            <Code className="w-6 h-6 animate-bounce" />
            <span>Full Stack Developer</span>
            <Cloud className="w-6 h-6 animate-bounce delay-100" />
            <span>Cloud Architect</span>
            <Rocket className="w-6 h-6 animate-bounce delay-200" />
            <span>AI Innovator</span>
          </div>
          <div className="flex justify-center gap-4">
            <a href="https://github.com/saketh-04" className="p-3 bg-purple-600 rounded-full hover:bg-purple-500 transition-all hover:scale-110 transform">
              <Github className="w-6 h-6" />
            </a>
            <a href="https://www.linkedin.com/in/paggilla-saketh-95a84833b/" className="p-3 bg-blue-600 rounded-full hover:bg-blue-500 transition-all hover:scale-110 transform">
              <Linkedin className="w-6 h-6" />
            </a>
            <a href="mailto:sakethpaggila666@gmail.com" className="p-3 bg-pink-600 rounded-full hover:bg-pink-500 transition-all hover:scale-110 transform">
              <Mail className="w-6 h-6" />
            </a>
          </div>
        </div>

        {/* Stats Cards */}
        <div className="grid grid-cols-1 md:grid-cols-4 gap-6 mb-16">
          {stats.map((stat, idx) => (
            <div
              key={idx}
              className="bg-white/5 backdrop-blur-lg rounded-2xl p-6 border border-purple-500/20 hover:border-purple-500/50 transition-all hover:scale-105 transform hover:shadow-2xl hover:shadow-purple-500/20"
              style={{ animationDelay: `${idx * 100}ms` }}
            >
              <div className={`w-12 h-12 rounded-xl bg-gradient-to-br ${stat.color} flex items-center justify-center mb-4`}>
                <stat.icon className="w-6 h-6 text-white" />
              </div>
              <div className="text-3xl font-bold mb-2">{stat.value}</div>
              <div className="text-purple-300 text-sm">{stat.label}</div>
            </div>
          ))}
        </div>

        {/* Snake Game Section */}
        <div className="mb-16 bg-white/5 backdrop-blur-lg rounded-3xl p-8 border border-purple-500/20">
          <div className="flex items-center justify-between mb-6">
            <h2 className="text-3xl font-bold flex items-center gap-3">
              <Terminal className="w-8 h-8 text-green-400" />
              🐍 Play Snake Game
            </h2>
            <div className="text-2xl font-bold text-yellow-400">Score: {score}</div>
          </div>
          <div className="flex flex-col md:flex-row gap-8">
            <div
              ref={gameRef}
              className="bg-black/50 rounded-xl border-2 border-green-500/50 overflow-hidden"
              style={{ width: '400px', height: '400px' }}
            >
              {!gameActive ? (
                <div className="w-full h-full flex items-center justify-center">
                  <button
                    onClick={startGame}
                    className="px-8 py-4 bg-gradient-to-r from-green-500 to-emerald-500 rounded-xl font-bold text-xl hover:scale-110 transform transition-all shadow-lg hover:shadow-green-500/50"
                  >
                    Start Game
                  </button>
                </div>
              ) : (
                <div className="relative w-full h-full">
                  {snake.map((segment, idx) => (
                    <div
                      key={idx}
                      className="absolute bg-gradient-to-br from-green-400 to-emerald-500 rounded-sm"
                      style={{
                        left: `${segment[1] * 5}%`,
                        top: `${segment[0] * 5}%`,
                        width: '5%',
                        height: '5%'
                      }}
                    />
                  ))}
                  <div
                    className="absolute bg-red-500 rounded-full animate-pulse"
                    style={{
                      left: `${food[1] * 5}%`,
                      top: `${food[0] * 5}%`,
                      width: '5%',
                      height: '5%'
                    }}
                  />
                </div>
              )}
            </div>
            <div className="flex-1 flex flex-col justify-center">
              <h3 className="text-2xl font-bold mb-4 text-green-400">How to Play:</h3>
              <ul className="space-y-3 text-lg text-purple-200">
                <li className="flex items-center gap-3">
                  <Zap className="w-5 h-5 text-yellow-400" />
                  Use arrow keys to control the snake
                </li>
                <li className="flex items-center gap-3">
                  <Zap className="w-5 h-5 text-yellow-400" />
                  Eat the red food to grow longer
                </li>
                <li className="flex items-center gap-3">
                  <Zap className="w-5 h-5 text-yellow-400" />
                  Don't hit the walls or yourself!
                </li>
                <li className="flex items-center gap-3">
                  <Zap className="w-5 h-5 text-yellow-400" />
                  Try to beat your high score!
                </li>
              </ul>
            </div>
          </div>
        </div>

        {/* Tech Stack with Animated Bars */}
        <div className="mb-16">
          <h2 className="text-4xl font-bold mb-8 text-center">💻 Tech Stack Mastery</h2>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            {techStack.map((tech, idx) => (
              <div key={idx} className="bg-white/5 backdrop-blur-lg rounded-2xl p-6 border border-purple-500/20 hover:border-purple-500/50 transition-all">
                <div className="flex justify-between mb-3">
                  <span className="font-bold text-lg">{tech.name}</span>
                  <span className="text-purple-300">{tech.level}%</span>
                </div>
                <div className="w-full bg-gray-700 rounded-full h-4 overflow-hidden">
                  <div
                    className={`h-full ${tech.color} rounded-full transition-all duration-1000 ease-out animate-width`}
                    style={{ width: `${tech.level}%` }}
                  />
                </div>
              </div>
            ))}
          </div>
        </div>

        {/* Featured Projects */}
        <div className="mb-16">
          <h2 className="text-4xl font-bold mb-8 text-center">🚀 Featured Projects</h2>
          <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
            {projects.map((project, idx) => (
              <div
                key={idx}
                className="group bg-white/5 backdrop-blur-lg rounded-2xl p-6 border border-purple-500/20 hover:border-purple-500/50 transition-all hover:scale-105 transform hover:shadow-2xl hover:shadow-purple-500/20 cursor-pointer"
              >
                <div className={`w-full h-40 rounded-xl bg-gradient-to-br ${project.gradient} mb-4 flex items-center justify-center text-5xl group-hover:scale-110 transition-transform`}>
                  {project.title.split(' ')[0]}
                </div>
                <h3 className="text-xl font-bold mb-2">{project.title}</h3>
                <p className="text-purple-300 mb-4">{project.desc}</p>
                <div className="flex flex-wrap gap-2">
                  {project.tech.map((t, i) => (
                    <span key={i} className="px-3 py-1 bg-purple-600/30 rounded-full text-sm">
                      {t}
                    </span>
                  ))}
                </div>
              </div>
            ))}
          </div>
        </div>

        {/* About Me - Animated Text */}
        <div className="text-center bg-white/5 backdrop-blur-lg rounded-3xl p-12 border border-purple-500/20">
          <h2 className="text-4xl font-bold mb-8">👨‍💻 About Me</h2>
          <p className="text-xl leading-relaxed text-purple-200 max-w-4xl mx-auto mb-6">
            I'm a passionate <span className="text-cyan-400 font-bold">Full Stack Developer</span> and{' '}
            <span className="text-pink-400 font-bold">Cloud Architect</span> from Hyderabad, India 🇮🇳. 
            I love building <span className="text-yellow-400 font-bold">scalable systems</span> and creating{' '}
            <span className="text-green-400 font-bold">beautiful user experiences</span>. Currently working on AI-powered 
            applications and contributing to open source. When I'm not coding, you'll find me exploring new technologies 
            or debugging with console.log() (and I'm not ashamed! 😎)
          </p>
          <div className="inline-block px-8 py-3 bg-gradient-to-r from-purple-600 to-pink-600 rounded-full font-bold text-lg hover:scale-110 transform transition-all shadow-lg hover:shadow-purple-500/50 cursor-pointer">
            Let's Build Something Amazing! 🚀
          </div>
        </div>
      </div>

      <style jsx>{`
        @keyframes gradient {
          0%, 100% { background-position: 0% 50%; }
          50% { background-position: 100% 50%; }
        }
        .animate-gradient {
          background-size: 200% 200%;
          animation: gradient 3s ease infinite;
        }
        @keyframes pulse-slow {
          0%, 100% { transform: scale(1); }
          50% { transform: scale(1.05); }
        }
        .animate-pulse-slow {
          animation: pulse-slow 3s ease-in-out infinite;
        }
        @keyframes fade-in {
          from { opacity: 0; transform: translateY(-20px); }
          to { opacity: 1; transform: translateY(0); }
        }
        .animate-fade-in {
          animation: fade-in 1s ease-out;
        }
        .delay-100 { animation-delay: 100ms; }
        .delay-200 { animation-delay: 200ms; }
      `}</style>
    </div>
  );
};

export default GitHubProfile;
