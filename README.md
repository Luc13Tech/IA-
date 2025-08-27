<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LandBeninAI - Assistant Foncier Intelligent</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #2e584c;
            --secondary-color: #4c9688;
            --accent-color: #f0c040;
            --light-color: #f8f8f8;
            --dark-color: #333;
            --benin-green: #008751;
            --benin-yellow: #fcd116;
            --benin-red: #e8112d;
            --ai-blue: #2563eb;
            --ai-purple: #7c3aed;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: var(--light-color);
            color: var(--dark-color);
            line-height: 1.6;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            position: relative;
            overflow-x: hidden;
        }
        
        body::before {
            content: "";
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-image: url('https://upload.wikimedia.org/wikipedia/commons/thumb/0/0a/Flag_of_Benin.svg/1200px-Flag_of_Benin.svg.png');
            background-size: cover;
            background-position: center;
            opacity: 0.03;
            z-index: -1;
        }
        
        .benin-logo-bg {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 500px;
            height: 500px;
            background-image: url('https://upload.wikimedia.org/wikipedia/commons/thumb/6/6f/Coat_of_arms_of_Benin.svg/1200px-Coat_of_arms_of_Benin.svg.png');
            background-size: contain;
            background-repeat: no-repeat;
            background-position: center;
            opacity: 0.03;
            z-index: -1;
            pointer-events: none;
        }
        
        header {
            background: linear-gradient(to right, var(--benin-green), var(--benin-yellow), var(--benin-red));
            color: white;
            padding: 1rem;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
        }
        
        nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
            max-width: 1200px;
            margin: 0 auto;
        }
        
        .logo {
            display: flex;
            align-items: center;
            font-size: 1.5rem;
            font-weight: bold;
        }
        
        .logo img {
            height: 40px;
            margin-right: 10px;
        }
        
        .nav-links {
            display: flex;
            list-style: none;
            align-items: center;
        }
        
        .nav-links li {
            margin-left: 1.5rem;
        }
        
        .nav-links a {
            color: white;
            text-decoration: none;
            transition: color 0.3s;
            font-weight: 500;
        }
        
        .nav-links a:hover {
            color: var(--accent-color);
        }
        
        .language-selector {
            display: flex;
            align-items: center;
            background: rgba(255, 255, 255, 0.15);
            border-radius: 20px;
            padding: 0.4rem 0.8rem;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .language-selector:hover {
            background: rgba(255, 255, 255, 0.25);
        }
        
        .language-selector span {
            margin-left: 0.5rem;
            font-size: 0.9rem;
        }
        
        .language-dropdown {
            position: absolute;
            top: 100%;
            right: 0;
            background: white;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            padding: 0.5rem;
            display: none;
            z-index: 101;
            min-width: 150px;
        }
        
        .language-dropdown.show {
            display: block;
        }
        
        .language-option {
            padding: 0.5rem 1rem;
            border-radius: 6px;
            cursor: pointer;
            transition: background-color 0.2s;
            display: flex;
            align-items: center;
        }
        
        .language-option:hover {
            background-color: #f3f4f6;
        }
        
        .language-option i {
            margin-right: 0.5rem;
            width: 20px;
            text-align: center;
        }
        
        .mobile-menu-btn {
            display: none;
            background: none;
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
        }
        
        .main-container {
            display: flex;
            flex: 1;
            max-width: 1400px;
            margin: 0 auto;
            width: 100%;
            padding: 1rem;
        }
        
        .sidebar {
            width: 300px;
            background: white;
            padding: 1.5rem;
            border-right: 1px solid #e5e7eb;
            display: flex;
            flex-direction: column;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
            margin-right: 1rem;
        }
        
        .sidebar h2 {
            color: var(--primary-color);
            margin-bottom: 1.5rem;
            padding-bottom: 0.5rem;
            border-bottom: 2px solid var(--accent-color);
        }
        
        .chat-history {
            flex: 1;
            overflow-y: auto;
            margin-bottom: 1rem;
        }
        
        .history-item {
            padding: 0.75rem;
            margin-bottom: 0.5rem;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.2s;
            display: flex;
            align-items: center;
        }
        
        .history-item:hover {
            background-color: #f3f4f6;
        }
        
        .history-item i {
            margin-right: 0.5rem;
            color: var(--secondary-color);
        }
        
        .new-chat-btn {
            background: linear-gradient(to right, var(--benin-green), var(--secondary-color));
            color: white;
            border: none;
            padding: 0.75rem 1rem;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-top: 1rem;
        }
        
        .new-chat-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        .new-chat-btn i {
            margin-right: 0.5rem;
        }
        
        .chat-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            background-color: white;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
            overflow: hidden;
        }
        
        .chat-header {
            padding: 1.5rem;
            border-bottom: 1px solid #e5e7eb;
            display: flex;
            align-items: center;
            background: linear-gradient(to right, #f8fafc, #f1f5f9);
        }
        
        .chat-header img {
            width: 40px;
            height: 40px;
            margin-right: 12px;
            border-radius: 50%;
        }
        
        .chat-header h1 {
            font-size: 1.5rem;
            color: var(--primary-color);
        }
        
        .chat-header p {
            color: var(--dark-color);
            opacity: 0.7;
            font-size: 0.9rem;
        }
        
        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 1.5rem;
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
            background-color: #fafafa;
        }
        
        .message {
            max-width: 80%;
            padding: 1rem 1.25rem;
            border-radius: 12px;
            animation: fadeIn 0.3s ease;
            position: relative;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .user-message {
            background: linear-gradient(135deg, var(--ai-blue), var(--ai-purple));
            color: white;
            align-self: flex-end;
            border-bottom-right-radius: 4px;
        }
        
        .bot-message {
            background-color: white;
            border: 1px solid #e5e7eb;
            align-self: flex-start;
            border-bottom-left-radius: 4px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
        }
        
        .message-typing {
            display: flex;
            align-items: center;
            padding: 0.5rem 1rem;
            background-color: white;
            border-radius: 12px;
            align-self: flex-start;
            margin-bottom: 1rem;
            border: 1px solid #e5e7eb;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
        }
        
        .typing-dots {
            display: flex;
            gap: 4px;
        }
        
        .typing-dot {
            width: 8px;
            height: 8px;
            background-color: var(--secondary-color);
            border-radius: 50%;
            animation: typingAnimation 1.4s infinite ease-in-out;
        }
        
        .typing-dot:nth-child(1) { animation-delay: 0s; }
        .typing-dot:nth-child(2) { animation-delay: 0.2s; }
        .typing-dot:nth-child(3) { animation-delay: 0.4s; }
        
        @keyframes typingAnimation {
            0%, 60%, 100% { transform: translateY(0); }
            30% { transform: translateY(-5px); }
        }
        
        .chat-input-container {
            padding: 1.5rem;
            border-top: 1px solid #e5e7eb;
            background-color: #f9fafb;
        }
        
        .chat-input-box {
            display: flex;
            gap: 12px;
            background-color: white;
            border: 1px solid #e5e7eb;
            border-radius: 12px;
            padding: 0.5rem;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
        }
        
        .chat-input {
            flex: 1;
            padding: 0.75rem;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            resize: none;
            height: 50px;
            max-height: 150px;
            outline: none;
        }
        
        .send-btn {
            background: linear-gradient(135deg, var(--benin-green), var(--secondary-color));
            color: white;
            border: none;
            width: 50px;
            height: 50px;
            border-radius: 8px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }
        
        .send-btn:hover {
            transform: scale(1.05);
        }
        
        .suggestions {
            display: flex;
            flex-wrap: wrap;
            gap: 0.75rem;
            margin-top: 1rem;
        }
        
        .suggestion-chip {
            background-color: white;
            border: 1px solid #e5e7eb;
            padding: 0.5rem 1rem;
            border-radius: 20px;
            font-size: 0.9rem;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .suggestion-chip:hover {
            background-color: #f3f4f6;
            transform: translateY(-2px);
        }
        
        footer {
            background: linear-gradient(to right, var(--benin-green), var(--primary-color));
            color: white;
            padding: 2rem 1rem;
            margin-top: auto;
        }
        
        .footer-content {
            max-width: 1200px;
            margin: 0 auto;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 2rem;
        }
        
        .footer-section h3 {
            margin-bottom: 1rem;
            position: relative;
            padding-bottom: 10px;
        }
        
        .footer-section h3:after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 40px;
            height: 3px;
            background-color: var(--accent-color);
        }
        
        .footer-section p, .footer-section li {
            margin-bottom: 0.5rem;
            font-size: 0.9rem;
        }
        
        .footer-section ul {
            list-style: none;
        }
        
        .footer-section a {
            color: #ddd;
            text-decoration: none;
            transition: color 0.3s;
        }
        
        .footer-section a:hover {
            color: var(--accent-color);
        }
        
        .social-icons {
            display: flex;
            gap: 12px;
            margin-top: 1rem;
        }
        
        .social-icons a {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            width: 36px;
            height: 36px;
            background-color: rgba(255, 255, 255, 0.1);
            border-radius: 50%;
            transition: all 0.3s;
        }
        
        .social-icons a:hover {
            background-color: var(--accent-color);
            color: var(--dark-color);
            transform: translateY(-3px);
        }
        
        .copyright {
            text-align: center;
            margin-top: 2rem;
            padding-top: 1.5rem;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            font-size: 0.85rem;
        }
        
        @media (max-width: 968px) {
            .main-container {
                flex-direction: column;
            }
            
            .sidebar {
                width: 100%;
                border-right: none;
                border-bottom: 1px solid #e5e7eb;
                padding: 1rem;
                margin-right: 0;
                margin-bottom: 1rem;
            }
            
            .chat-history {
                max-height: 150px;
            }
        }
        
        @media (max-width: 768px) {
            .nav-links {
                display: none;
                position: absolute;
                top: 100%;
                left: 0;
                width: 100%;
                background: linear-gradient(to right, var(--benin-green), var(--primary-color));
                flex-direction: column;
                padding: 1rem;
                gap: 1rem;
                z-index: 100;
            }
            
            .nav-links.active {
                display: flex;
            }
            
            .mobile-menu-btn {
                display: block;
            }
            
            .message {
                max-width: 90%;
            }
            
            .chat-header h1 {
                font-size: 1.25rem;
            }
        }
        
        @media (max-width: 480px) {
            .chat-header {
                padding: 1rem;
            }
            
            .chat-messages {
                padding: 1rem;
            }
            
            .chat-input-container {
                padding: 1rem;
            }
            
            .suggestions {
                gap: 0.5rem;
            }
            
            .suggestion-chip {
                padding: 0.4rem 0.8rem;
                font-size: 0.8rem;
            }
            
            .benin-logo-bg {
                width: 300px;
                height: 300px;
            }
        }
    </style>
</head>
<body>
    <div class="benin-logo-bg"></div>
    
    <header>
        <nav>
            <div class="logo">
                <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/0a/Flag_of_Benin.svg/320px-Flag_of_Benin.svg.png" alt="Drapeau du Bénin">
                <span>LandBeninAI</span>
            </div>
            <ul class="nav-links" id="navLinks">
                <li><a href="#features">Fonctionnalités</a></li>
                <li><a href="#about">À propos</a></li>
                <li><a href="#contact">Contact</a></li>
                <li>
                    <div class="language-selector" id="languageSelector">
                        <i class="fas fa-globe"></i>
                        <span id="currentLanguage">Français</span>
                        <i class="fas fa-chevron-down" style="margin-left: 0.5rem; font-size: 0.8rem;"></i>
                    </div>
                    <div class="language-dropdown" id="languageDropdown">
                        <div class="language-option" data-lang="fr">
                            <i class="fas fa-check" id="checkFr"></i>
                            Français
                        </div>
                        <div class="language-option" data-lang="en">
                            <i class="fas fa-check" id="checkEn" style="display: none;"></i>
                            English
                        </div>
                        <div class="language-option" data-lang="es">
                            <i class="fas fa-check" id="checkEs" style="display: none;"></i>
                            Español
                        </div>
                        <div class="language-option" data-lang="pt">
                            <i class="fas fa-check" id="checkPt" style="display: none;"></i>
                            Português
                        </div>
                    </div>
                </li>
            </ul>
            <button class="mobile-menu-btn" id="menuBtn">
                <i class="fas fa-bars"></i>
            </button>
        </nav>
    </header>

    <div class="main-container">
        <aside class="sidebar">
            <h2 id="historyTitle">Historique des conversations</h2>
            <div class="chat-history" id="chatHistory">
                <div class="history-item">
                    <i class="fas fa-comment"></i>
                    <span id="history1">Questions sur les titres fonciers</span>
                </div>
                <div class="history-item">
                    <i class="fas fa-comment"></i>
                    <span id="history2">Litiges de propriété</span>
                </div>
                <div class="history-item">
                    <i class="fas fa-comment"></i>
                    <span id="history3">Procédure d'acquisition terrain</span>
                </div>
            </div>
            <button class="new-chat-btn" id="newChatBtn">
                <i class="fas fa-plus"></i>
                <span id="newChatText">Nouvelle conversation</span>
            </button>
        </aside>

        <main class="chat-container">
            <div class="chat-header">
                <img src="https://cdn-icons-png.flaticon.com/512/4711/4711986.png" alt="Assistant IA">
                <div>
                    <h1 id="chatbotTitle">Assistant Foncier LandBeninAI</h1>
                    <p id="chatbotSubtitle">Spécialiste des questions foncières au Bénin - Disponible 24h/24</p>
                </div>
            </div>

            <div class="chat-messages" id="chatMessages">
                <div class="message bot-message" id="welcomeMessage">
                    Bonjour ! Je suis votre assistant foncier intelligent. Je peux vous aider avec toutes vos questions concernant les terrains, les propriétés, les litiges fonciers et les procédures légales au Bénin. Comment puis-je vous aider aujourd'hui ?
                </div>
            </div>

            <div class="chat-input-container">
                <div class="chat-input-box">
                    <textarea class="chat-input" id="userInput" placeholder="Posez votre question sur le droit foncier au Bénin..." rows="1"></textarea>
                    <button class="send-btn" id="sendBtn">
                        <i class="fas fa-paper-plane"></i>
                    </button>
                </div>
                <div class="suggestions">
                    <div class="suggestion-chip" data-question="documents">
                        <span id="suggestion1">Documents pour enregistrement terrain</span>
                    </div>
                    <div class="suggestion-chip" data-question="litige">
                        <span id="suggestion2">Résolution litige foncier</span>
                    </div>
                    <div class="suggestion-chip" data-question="acquisition">
                        <span id="suggestion3">Procédure acquisition terrain</span>
                    </div>
                    <div class="suggestion-chip" data-question="titre">
                        <span id="suggestion4">Vérification titre foncier</span>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <footer id="contact">
        <div class="footer-content">
            <div class="footer-section">
                <h3 id="aboutTitle">À propos de LandBeninAI</h3>
                <p id="aboutText">Plateforme intelligente utilisant l'IA pour moderniser et sécuriser le système foncier au Bénin.</p>
                <div class="social-icons">
                    <a href="#"><i class="fab fa-facebook-f"></i></a>
                    <a href="#"><i class="fab fa-twitter"></i></a>
                    <a href="#"><i class="fab fa-linkedin-in"></i></a>
                    <a href="#"><i class="fab fa-instagram"></i></a>
                </div>
            </div>
            
            <div class="footer-section">
                <h3 id="linksTitle">Liens rapides</h3>
                <ul>
                    <li><a href="#">Accueil</a></li>
                    <li><a href="#">Fonctionnalités</a></li>
                    <li><a href="#">À propos</a></li>
                    <li><a href="#">Contact</a></li>
                </ul>
            </div>
            
            <div class="footer-section">
                <h3 id="contactTitle">Contactez-nous</h3>
                <p><i class="fas fa-map-marker-alt"></i> Cotonou, Bénin</p>
                <p><i class="fas fa-phone"></i> +229 XX XX XX XX</p>
                <p><i class="fas fa-envelope"></i> contact@landbeninai.bj</p>
            </div>
        </div>
        
        <div class="copyright">
            <p>&copy; 2023 LandBeninAI - <span id="rightsText">Tous droits réservés</span></p>
        </div>
    </footer>

    <script>
        // ===== CONFIGURATION DES LANGUES =====
        const translations = {
            fr: {
                currentLanguage: "Français",
                historyTitle: "Historique des conversations",
                history1: "Questions sur les titres fonciers",
                history2: "Litiges de propriété",
                history3: "Procédure d'acquisition terrain",
                newChatText: "Nouvelle conversation",
                chatbotTitle: "Assistant Foncier LandBeninAI",
                chatbotSubtitle: "Spécialiste des questions foncières au Bénin - Disponible 24h/24",
                welcomeMessage: "Bonjour ! Je suis votre assistant foncier intelligent. Je peux vous aider avec toutes vos questions concernant les terrains, les propriétés, les litiges fonciers et les procédures légales au Bénin. Comment puis-je vous aider aujourd'hui ?",
                inputPlaceholder: "Posez votre question sur le droit foncier au Bénin...",
                suggestion1: "Documents pour enregistrement terrain",
                suggestion2: "Résolution litige foncier",
                suggestion3: "Procédure acquisition terrain",
                suggestion4: "Vérification titre foncier",
                aboutTitle: "À propos de LandBeninAI",
                aboutText: "Plateforme intelligente utilisant l'IA pour moderniser et sécuriser le système foncier au Bénin.",
                linksTitle: "Liens rapides",
                contactTitle: "Contactez-nous",
                rightsText: "Tous droits réservés"
            },
            en: {
                currentLanguage: "English",
                historyTitle: "Conversation History",
                history1: "Questions about land titles",
                history2: "Property disputes",
                history3: "Land acquisition process",
                newChatText: "New conversation",
                chatbotTitle: "LandBeninAI Legal Assistant",
                chatbotSubtitle: "Specialist in land issues in Benin - Available 24/7",
                welcomeMessage: "Hello! I am your intelligent land assistant. I can help you with all your questions regarding land, properties, land disputes and legal procedures in Benin. How can I help you today?",
                inputPlaceholder: "Ask your question about land law in Benin...",
                suggestion1: "Documents for land registration",
                suggestion2: "Resolving land disputes",
                suggestion3: "Land acquisition procedure",
                suggestion4: "Verifying land title",
                aboutTitle: "About LandBeninAI",
                aboutText: "Intelligent platform using AI to modernize and secure the land system in Benin.",
                linksTitle: "Quick Links",
                contactTitle: "Contact Us",
                rightsText: "All rights reserved"
            },
            es: {
                currentLanguage: "Español",
                historyTitle: "Historial de conversaciones",
                history1: "Preguntas sobre títulos de propiedad",
                history2: "Disputas de propiedad",
                history3: "Procedimiento de adquisición de terrenos",
                newChatText: "Nueva conversación",
                chatbotTitle: "Asistente Legal LandBeninAI",
                chatbotSubtitle: "Especialista en cuestiones de tierras en Benin - Disponible 24/7",
                welcomeMessage: "¡Hola! Soy tu asistente de tierras inteligente. Puedo ayudarte con todas tus preguntas sobre terrenos, propiedades, disputas de tierras y procedimientos legales en Benin. ¿Cómo puedo ayudarte hoy?",
                inputPlaceholder: "Haga su pregunta sobre derecho de la tierra en Benin...",
                suggestion1: "Documentos para registro de tierras",
                suggestion2: "Resolución de disputas de tierras",
                suggestion3: "Procedimiento de adquisición de tierras",
                suggestion4: "Verificación de título de propiedad",
                aboutTitle: "Acerca de LandBeninAI",
                aboutText: "Plataforma inteligente que utiliza IA para modernizar y asegurar el sistema de tierras en Benin.",
                linksTitle: "Enlaces Rápidos",
                contactTitle: "Contáctenos",
                rightsText: "Todos los derechos reservados"
            },
            pt: {
                currentLanguage: "Português",
                historyTitle: "Histórico de conversas",
                history1: "Perguntas sobre títulos de terra",
                history2: "Disputas de propriedade",
                history3: "Processo de aquisição de terras",
                newChatText: "Nova conversa",
                chatbotTitle: "Assistente Legal LandBeninAI",
                chatbotSubtitle: "Especialista em questões fundiárias no Benin - Disponível 24/7",
                welcomeMessage: "Olá! Sou seu assistente de terras inteligente. Posso ajudá-lo com todas as suas perguntas sobre terrenos, propriedades, disputas de terra e procedimentos legais no Benin. Como posso ajudá-lo hoje?",
                inputPlaceholder: "Faça sua pergunta sobre direito fundiário no Benin...",
                suggestion1: "Documentos para registro de terra",
                suggestion2: "Resolução de disputas de terra",
                suggestion3: "Procedimento de aquisição de terra",
                suggestion4: "Verificação de título de terra",
                aboutTitle: "Sobre LandBeninAI",
                aboutText: "Plataforma inteligente que usa IA para modernizar e proteger o sistema de terras no Benin.",
                linksTitle: "Links Rápidos",
                contactTitle: "Contate-Nos",
                rightsText: "Todos os direitos reservados"
            }
        };

        // ===== BASE DE CONNAISSANCES MULTILINGUE =====
        const knowledgeBase = {
            "documents": {
                patterns: ["documents", "papiers", "dossier", "pièces", "documents", "papers", "dossier", "documentos", "papeles", "documentos"],
                responses: {
                    fr: [
                        "Pour enregistrer un terrain au Bénin, les documents requis sont : 1) Acte de vente notarié, 2) Titre foncier ou attestation de possession, 3) Plan de situation et de délimitation, 4) Certificat de non-opposition, 5) Reçu de paiement des taxes. Je peux vous fournir plus de détails sur chacun de ces documents si nécessaire.",
                        "L'enregistrement d'un terrain au Bénin nécessite plusieurs documents essentiels : un acte de vente notarié, le titre foncier, un plan de délimitation, un certificat de non-opposition et le reçu de paiement des taxes. Chacun de ces documents joue un rôle crucial dans le processus.",
                        "Les documents requis pour l'enregistrement foncier incluent : acte de vente notarié, titre de propriété, plan de situation, certificat de non-opposition et quittance fiscale. Des documents supplémentaires peuvent être demandés selon la nature de la transaction."
                    ],
                    en: [
                        "To register land in Benin, the required documents are: 1) Notarized deed of sale, 2) Land title or possession certificate, 3) Location and demarcation plan, 4) Non-opposition certificate, 5) Tax payment receipt. I can provide more details on each of these documents if needed.",
                        "Land registration in Benin requires several essential documents: a notarized deed of sale, land title, demarcation plan, non-opposition certificate, and tax payment receipt. Each of these documents plays a crucial role in the process.",
                        "Required documents for land registration include: notarized deed of sale, property title, location plan, non-opposition certificate and tax receipt. Additional documents may be required depending on the nature of the transaction."
                    ],
                    es: [
                        "Para registrar un terreno en Benin, los documentos requeridos son: 1) Escritura de venta notariada, 2) Título de propiedad o certificado de posesión, 3) Plan de situación y delimitación, 4) Certificado de no oposición, 5) Recibo de pago de impuestos. Puedo proporcionar más detalles sobre cada uno de estos documentos si es necesario.",
                        "El registro de tierras en Benin requiere varios documentos esenciales: una escritura de venta notariada, título de propiedad, plan de demarcación, certificado de no oposición y recibo de pago de impuestos. Cada uno de estos documentos juega un papel crucial en el proceso.",
                        "Los documentos requeridos para el registro de propiedad incluyen: escritura de venta notariada, título de propiedad, plan de ubicación, certificado de no oposición y recibo de impuestos. Pueden requerirse documentos adicionales según la naturaleza de la transacción."
                    ],
                    pt: [
                        "Para registrar um terreno no Benin, os documentos necessários são: 1) Escritura de venda notariada, 2) Título de propriedade ou certificado de posse, 3) Plano de situação e demarcação, 4) Certificado de não oposição, 5) Recibo de pagamento de impostos. Posso fornecer mais detalhes sobre cada um desses documentos, se necessário.",
                        "O registro de terras no Benin requer vários documentos essenciais: uma escritura de venda notariada, título de propriedade, plano de demarcação, certificado de não oposição e recibo de pagamento de impostos. Cada um desses documentos desempenha um papel crucial no processo.",
                        "Os documentos necessários para o registro de terras incluem: escritura de venda notariada, título de propriedade, plano de localização, certificado de não oposição e recibo de impostos. Documentos adicionais podem ser necessários dependendo da natureza da transação."
                    ]
                }
            },
            "litige": {
                patterns: ["litige", "conflit", "dispute", "problème", "contestation", "dispute", "conflict", "problem", "contention", "disputa", "conflicto", "problema", "contestação", "conflito"],
                responses: {
                    fr: [
                        "En cas de litige foncier au Bénin, la procédure recommandée est : 1) Tentative de médiation avec le conciliateur de justice, 2) Saisie du tribunal de première instance, 3) Expertise technique si nécessaire. La nouvelle loi foncière de 2017 prévoit des mécanismes spécifiques de résolution des conflits. Souhaitez-vous que je détaille une de ces étapes ?",
                        "Pour résoudre un litige foncier, il est conseillé de d'abord tenter une médiation, puis si nécessaire, de saisir le tribunal compétent. La loi foncière béninoise de 2017 a introduit des processus de résolution alternatifs des conflits qui peuvent être plus rapides que les procédures judiciaires traditionnelles.",
                        "Les litiges fonciers peuvent être complexes. La procédure commence généralement par une tentative de conciliation, suivie si nécessaire d'une action en justice. Il est important de recueillir tous les documents prouvant vos droits sur la propriété avant d'engager toute procédure."
                    ],
                    en: [
                        "In case of land dispute in Benin, the recommended procedure is: 1) Attempt mediation with the justice conciliator, 2) Referral to the court of first instance, 3) Technical expertise if necessary. The new land law of 2017 provides specific conflict resolution mechanisms. Would you like me to detail one of these steps?",
                        "To resolve a land dispute, it is advisable to first attempt mediation, then if necessary, refer the matter to the competent court. The Beninese land law of 2017 introduced alternative conflict resolution processes that can be faster than traditional legal procedures.",
                        "Land disputes can be complex. The procedure usually begins with an attempt at conciliation, followed if necessary by legal action. It is important to gather all documents proving your rights to the property before initiating any procedure."
                    ],
                    es: [
                        "En caso de disputa de tierra en Benin, el procedimiento recomendado es: 1) Intentar la mediación con el conciliador de justicia, 2) Remisión al tribunal de primera instancia, 3) Experticia técnica si es necesario. La nueva ley de tierras de 2017 prevé mecanismos específicos de resolución de conflictos. ¿Le gustaría que detalle uno de estos pasos?",
                        "Para resolver una disputa de tierra, se recomienda primero intentar la mediación, luego si es necesario, remitir el asunto al tribunal competente. La ley de tierras beninesa de 2017 introdujo procesos alternativos de resolución de conflictos que pueden ser más rápidos que los procedimientos legales tradicionales.",
                        "Las disputas de tierra pueden ser complejas. El procedimiento generalmente comienza con un intento de conciliación, seguido si es necesario por acción legal. Es importante recopilar todos los documentos que prueben sus derechos sobre la propiedad antes de iniciar cualquier procedimiento."
                    ],
                    pt: [
                        "Em caso de disputa de terra no Benin, o procedimento recomendado é: 1) Tentativa de mediação com o conciliador de justiça, 2) Encaminhamento ao tribunal de primeira instância, 3) Perícia técnica se necessário. A nova lei de terras de 2017 prevê mecanismos específicos de resolução de conflitos. Gostaria que detalhe uma dessas etapas?",
                        "Para resolver uma disputa de terra, é aconselhável primeiro tentar a mediação, depois se necessário, encaminhar o assunto ao tribunal competente. A lei de terras beninense de 2017 introduziu processos alternativos de resolução de conflitos que podem ser mais rápidos do que os procedimentos legais tradicionais.",
                        "Disputas de terra podem ser complexas. O procedimento geralmente começa com uma tentativa de conciliação, seguida se necessário por ação legal. É importante reunir todos os documentos que comprovem seus direitos sobre a propriedade antes de iniciar qualquer procedimento."
                    ]
                }
            },
            "acquisition": {
                patterns: ["acheter", "acquisition", "acquérir", "achete", "procedure", "buy", "purchase", "acquire", "procedure", "comprar", "adquisición", "adquirir", "procedimiento", "comprar", "aquisição", "adquirir", "procedimento"],
                responses: {
                    fr: [
                        "La procédure d'acquisition d'un terrain au Bénin suit plusieurs étapes : 1) Vérification de la disponibilité et du statut juridique du terrain, 2) Signature d'un compromis de vente, 3) Établissement de l'acte de vente devant notaire, 4) Enregistrement au service foncier, 5) Publication au livre foncier. Le processus complet prend généralement 3 à 6 mois.",
                        "Acquérir un terrain au Bénin nécessite de suivre une procédure précise : vérification du titre de propriété, signature d'un avant-contrat, établissement de l'acte authentique chez le notaire, enregistrement au service des domaines et publication au livre foncier. Chaque étape est importante pour garantir la sécurité juridique de la transaction.",
                        "L'acquisition d'un terrain implique plusieurs phases cruciales : due diligence sur le statut juridique du terrain, négociation des termes, signature devant notaire et enregistrement officiel. Il est recommandé de faire appel à un notaire pour accompagner l'ensemble du processus."
                    ],
                    en: [
                        "The procedure for acquiring land in Benin follows several steps: 1) Verification of availability and legal status of the land, 2) Signing of a preliminary sales agreement, 3) Establishment of the deed of sale before a notary, 4) Registration with the land service, 5) Publication in the land register. The complete process generally takes 3 to 6 months.",
                        "Acquiring land in Benin requires following a precise procedure: verification of the property title, signing of a preliminary contract, establishment of the authentic deed with the notary, registration with the domains service and publication in the land register. Each step is important to guarantee the legal security of the transaction.",
                        "Land acquisition involves several crucial phases: due diligence on the legal status of the land, negotiation of terms, signing before a notary and official registration. It is recommended to use a notary to accompany the entire process."
                    ],
                    es: [
                        "El procedimiento para adquirir un terreno en Benin sigue varios pasos: 1) Verificación de la disponibilidad y estado legal del terreno, 2) Firma de un acuerdo preliminar de venta, 3) Establecimiento de la escritura de venta ante notario, 4) Registro en el servicio de tierras, 5) Publicación en el registro de la propiedad. El proceso completo generalmente toma de 3 a 6 meses.",
                        "Adquirir un terreno en Benin requiere seguir un procedimiento preciso: verificación del título de propiedad, firma de un contrato preliminar, establecimiento de la escritura auténtica con el notario, registro en el servicio de dominios y publicación en el registro de la propiedad. Cada paso es importante para garantizar la seguridad jurídica de la transacción.",
                        "La adquisición de terrenos implica varias fases cruciales: due diligence sobre el estado legal del terreno, negociación de términos, firma ante notario y registro oficial. Se recomienda utilizar un notario para acompañar todo el proceso."
                    ],
                    pt: [
                        "O procedimento para aquisição de terra no Benin segue várias etapas: 1) Verificação da disponibilidade e status legal do terreno, 2) Assinatura de um acordo preliminar de venda, 3) Estabelecimento da escritura de venda perante notário, 4) Registro no serviço de terras, 5) Publicação no registro de propriedade. O processo completo geralmente leva de 3 a 6 meses.",
                        "Adquirir terra no Benin requer seguir um procedimento preciso: verificação do título de propriedade, assinatura de contrato preliminar, estabelecimento da escritura autêntica com o notário, registro no serviço de domínios e publicação no registro de propriedade. Cada etapa é importante para garantir a segurança jurídica da transação.",
                        "A aquisição de terras envolve várias fases cruciais: due diligence sobre o status legal do terreno, negociação de termos, assinatura perante notário e registro oficial. Recomenda-se usar um notário para acompanhar todo o processo."
                    ]
                }
            },
            "titre": {
                patterns: ["titre", "authenticité", "vérifier", "vrai", "faux", "title", "authenticity", "verify", "real", "fake", "título", "autenticidad", "verificar", "verdadero", "falso", "título", "autenticidade", "verificar", "verdadeiro", "falso"],
                responses: {
                    fr: [
                        "Pour vérifier l'authenticité d'un titre foncier au Bénin, vous devez : 1) Consulter le service départemental du domaine et du foncier, 2) Vérifier le numéro d'enregistrement au livre foncier, 3) Contrôler la concordance des informations avec le cadastre. Depuis 2020, un système numérique permet de faire certaines vérifications en ligne via le guichet unique foncier.",
                        "La vérification d'un titre foncier implique de confirmer son enregistrement auprès des services compétents et de s'assurer de la correspondance entre les informations du titre et celles du cadastre. Il est également important de vérquer l'historique des transactions pour détecter d'éventuelles irrégularités.",
                        "Authentifier un titre foncier nécessite plusieurs vérifications : consultation au service foncier, examen du numéro d'enregistrement, comparaison avec les données cadastrales. Les titres délivrés après 2017 bénéficient d'un système de numérotation sécurisé qui rend la falsification plus difficile."
                    ],
                    en: [
                        "To verify the authenticity of a land title in Benin, you must: 1) Consult the departmental domain and land service, 2) Check the registration number in the land register, 3) Control the consistency of information with the cadastre. Since 2020, a digital system allows some checks online via the one-stop land shop.",
                        "Verifying a land title involves confirming its registration with the competent services and ensuring the correspondence between the title information and the cadastre. It is also important to check the transaction history to detect any irregularities.",
                        "Authenticating a land title requires several checks: consultation with the land service, examination of the registration number, comparison with cadastral data. Titles issued after 2017 benefit from a secure numbering system that makes falsification more difficult."
                    ],
                    es: [
                        "Para verificar la autenticidad de un título de propiedad en Benin, debe: 1) Consultar el servicio departamental de dominio y tierras, 2) Verificar el número de registro en el registro de la propiedad, 3) Controlar la concordancia de la información con el catastro. Desde 2020, un sistema digital permite realizar algunas verificaciones en línea a través de la ventanilla única de tierras.",
                        "Verificar un título de propiedad implica confirmar su registro ante los servicios competentes y asegurar la correspondencia entre la información del título y el catastro. También es importante verificar el historial de transacciones para detectar posibles irregularidades.",
                        "Autenticar un título de propiedad requiere varias verificaciones: consulta con el servicio de tierras, examen del número de registro, comparación con datos catastrales. Los títulos emitidos después de 2017 se benefician de un sistema de numeración seguro que dificulta la falsificación."
                    ],
                    pt: [
                        "Para verificar a autenticidade de um título de propriedade no Benin, você deve: 1) Consultar o serviço departamental de domínio e terras, 2) Verificar o número de registro no registro de propriedade, 3) Controlar a concordância das informações com o cadastro. Desde 2020, um sistema digital permite algumas verificações online através do balcão único de terras.",
                        "Verificar um título de propriedade envolve confirmar seu registro nos serviços competentes e garantir a correspondência entre as informações do título e o cadastro. Também é importante verificar o histórico de transações para detectar possíveis irregularidades.",
                        "Autenticar um título de propriedade requer várias verificações: consulta ao serviço de terras, exame do número de registro, comparação com dados cadastrais. Os títulos emitidos após 2017 beneficiam de um sistema de numeração seguro que dificulta a falsificação."
                    ]
                }
            },
            "prix": {
                patterns: ["prix", "coût", "frais", "taxe", "argent", "price", "cost", "fees", "tax", "money", "precio", "costo", "tarifa", "impuesto", "dinero", "preço", "custo", "taxa", "imposto", "dinheiro"],
                responses: {
                    fr: [
                        "Les coûts associés à l'acquisition d'un terrain au Bénin incluent : 1) Droits d'enregistrement (5% de la valeur vénale), 2) Honoraires de notaire (variable selon la transaction), 3) Taxe de publicité foncière (2%), 4) Frais de bornage et topographie. Le total des frais représente généralement 10 à 15% de la valeur du terrain.",
                        "L'acquisition d'un terrain génère plusieurs types de frais : droits d'enregistrement calculés sur la valeur vénale, honoraires notariaux, taxe de publicité foncière, et frais techniques de bornage. Il est important de prévoir un budget pour ces frais qui peuvent significativement augmenter le coût total.",
                        "Les frais liés à une transaction foncière comprennent les droits d'enregistrement (5%), les émoluments du notaire (généralement 1 à 2%), la taxe de publicité foncière (2%) et les frais de géomètre. Le total peut varier entre 8% et 12% du prix d'achat selon la complexité du dossier."
                    ],
                    en: [
                        "Costs associated with land acquisition in Benin include: 1) Registration fees (5% of the market value), 2) Notary fees (variable depending on the transaction), 3) Land publicity tax (2%), 4) Surveying and topography fees. The total fees generally represent 10 to 15% of the land value.",
                        "Land acquisition generates several types of fees: registration fees calculated on the market value, notary fees, land publicity tax, and technical surveying fees. It is important to budget for these fees which can significantly increase the total cost.",
                        "Fees related to a land transaction include registration fees (5%), notary fees (usually 1 to 2%), land publicity tax (2%) and surveyor fees. The total can vary between 8% and 12% of the purchase price depending on the complexity of the file."
                    ],
                    es: [
                        "Los costos asociados con la adquisición de tierra en Benin incluyen: 1) Derechos de registro (5% del valor venal), 2) Honorarios notariales (variables según la transacción), 3) Impuesto de publicidad de tierras (2%), 4) Gastos de demarcación y topografía. Los honorarios totales generalmente representan del 10 al 15% del valor de la tierra.",
                        "La adquisición de tierra genera varios tipos de gastos: derechos de registro calculados sobre el valor venal, honorarios notariales, impuesto de publicidad de tierras y gastos técnicos de demarcación. Es importante presupuestar estos gastos que pueden aumentar significativamente el costo total.",
                        "Los gastos relacionados con una transacción de tierras incluyen derechos de registro (5%), honorarios notariales (generalmente 1 a 2%), impuesto de publicidad de tierras (2%) y gastos de topógrafo. El total puede variar entre 8% y 12% del precio de compra según la complejidad del expediente."
                    ],
                    pt: [
                        "Os custos associados à aquisição de terra no Benin incluem: 1) Direitos de registro (5% do valor venal), 2) Honorários notariais (variáveis dependendo da transação), 3) Imposto de publicidade de terras (2%), 4) Despesas de demarcação e topografia. As taxas totais geralmente representam 10 a 15% do valor da terra.",
                        "A aquisição de terra gera vários tipos de despesas: direitos de registro calculados sobre o valor venal, honorários notariais, imposto de publicidade de terras e despesas técnicas de demarcação. É importante orçar essas despesas que podem aumentar significativamente o custo total.",
                        "As despesas relacionadas a uma transação de terra incluem direitos de registro (5%), honorários notariais (geralmente 1 a 2%), imposto de publicidade de terras (2%) e despesas de topógrafo. O total pode variar entre 8% e 12% do preço de compra dependendo da complexidade do processo."
                    ]
                }
            },
            "succession": {
                patterns: ["succession", "héritage", "hériter", "décès", "inheritance", "heritage", "inherit", "death", "sucesión", "herencia", "heredar", "muerte", "sucessão", "herança", "herdar", "morte"],
                responses: {
                    fr: [
                        "Pour régler une succession foncière au Bénin, les héritiers doivent : 1) Obtenir un certificat d'héritier ou un acte de notoriété, 2) Faire enregistrer la mutation au service des domaines, 3) Publier la mutation au livre foncier. Les droits de succession varient selon le degré de parenté et la valeur des biens.",
                        "Une succession implique le transfert des biens fonciers du défunt à ses héritiers. La procédure comprend l'obtention d'un acte de notoriété, la déclaration de succession, le paiement des droits de mutation et l'enregistrement au service foncier. Les héritiers directs bénéficient souvent d'exonérations partielles.",
                        "Le règlement d'une succession foncière nécessite de suivre plusieurs étapes administratives et juridiques. Il est recommandé de consulter un notaire pour s'assurer du respect des formalités et optimiser les droits à payer selon les liens de parenté avec le défunt."
                    ],
                    en: [
                        "To settle a land succession in Benin, heirs must: 1) Obtain a certificate of heirship or an act of notoriety, 2) Register the transfer with the domains service, 3) Publish the transfer in the land register. Inheritance taxes vary depending on the degree of relationship and the value of the assets.",
                        "An inheritance involves the transfer of the deceased's land assets to his heirs. The procedure includes obtaining an act of notoriety, declaring the inheritance, paying transfer duties and registering with the land service. Direct heirs often benefit from partial exemptions.",
                        "Settling a land inheritance requires following several administrative and legal steps. It is recommended to consult a notary to ensure compliance with formalities and optimize the fees to be paid according to the relationship with the deceased."
                    ],
                    es: [
                        "Para liquidar una sucesión de tierras en Benin, los herederos deben: 1) Obtener un certificado de heredero o un acta de notoriedad, 2) Registrar la transferencia en el servicio de dominios, 3) Publicar la transferencia en el registro de la propiedad. Los impuestos sucesorios varían según el grado de parentesco y el valor de los bienes.",
                        "Una sucesión implica la transferencia de los bienes inmuebles del difunto a sus herederos. El procedimiento incluye obtener un acta de notoriedad, declarar la sucesión, pagar los derechos de transferencia y registrarse en el servicio de tierras. Los herederos directos a menudo se benefician de exenciones parciales.",
                        "Liquidar una sucesión de tierras requiere seguir varios pasos administrativos y legales. Se recomienda consultar a un notario para asegurar el cumplimiento de las formalidades y optimizar los impuestos a pagar según la relación con el difunto."
                    ],
                    pt: [
                        "Para liquidar uma sucessão de terras no Benin, os herdeiros devem: 1) Obter um certificado de herdeiro ou uma ata de notoriedade, 2) Registrar a transferência no serviço de domínios, 3) Publicar a transferência no registro de propriedade. Os impostos sucessórios variam de acordo com o grau de parentesco e o valor dos bens.",
                        "Uma sucessão envolve a transferência dos bens imóveis do falecido para seus herdeiros. O procedimento inclui obter uma ata de notoriedade, declarar a sucessão, pagar os direitos de transferência e registrar-se no serviço de terras. Os herdeiros diretos frequentemente beneficiam de isenções parciais.",
                        "Liquidar uma sucessão de terras requer seguir várias etapas administrativas e legais. Recomenda-se consultar um notário para garantir o cumprimento das formalidades e otimizar os impostos a pagar de acordo com o relacionamento com o falecido."
                    ]
                }
            },
            "construction": {
                patterns: ["construction", "bâtir", "permis", "construire", "build", "construction", "permit", "build", "construcción", "edificar", "permiso", "construir", "construção", "edificar", "permissão", "construir"],
                responses: {
                    fr: [
                        "Avant de construire sur un terrain au Bénin, vous devez obtenir : 1) Un certificat d'urbanisme, 2) Un permis de construire délivré par la mairie, 3) Une autorisation d'occupation du sol si applicable. Les documents à fournir incluent le plan de situation, le plan de masse et les plans architecturaux.",
                        "La construction sur un terrain nécessite plusieurs autorisations préalables. Le permis de construire est obligatoire pour toute construction nouvelle et doit être obtenu avant le début des travaux. Le non-respect de cette obligation peut entraîner des sanctions et la démolition de la construction.",
                        "Pour obtenir un permis de construire, il faut déposer un dossier complet comprenant une demande, les plans de construction, une notice descriptive et l'attestation du terrain. La mairie a deux mois pour répondre. En l'absence de réponse dans ce délai, le permis est considéré comme accordé."
                    ],
                    en: [
                        "Before building on land in Benin, you must obtain: 1) A urban planning certificate, 2) A building permit issued by the town hall, 3) A land occupancy authorization if applicable. Documents to provide include the location plan, site plan and architectural plans.",
                        "Construction on land requires several prior authorizations. The building permit is mandatory for any new construction and must be obtained before the start of work. Non-compliance with this obligation can lead to sanctions and demolition of the construction.",
                        "To obtain a building permit, you must submit a complete file including an application, construction plans, a descriptive notice and the land certificate. The town hall has two months to respond. In the absence of a response within this period, the permit is considered granted."
                    ],
                    es: [
                        "Antes de construir en un terreno en Benin, debe obtener: 1) Un certificado de urbanismo, 2) Un permiso de construcción emitido por el ayuntamiento, 3) Una autorización de ocupación del suelo si corresponde. Los documentos a proporcionar incluyen el plan de ubicación, el plan de sitio y los planos arquitectónicos.",
                        "La construcción en un terreno requiere varias autorizaciones previas. El permiso de construcción es obligatorio para cualquier construcción nueva y debe obtenerse antes del inicio de los trabajos. El incumplimiento de esta obligación puede conllevar sanciones y la demolición de la construcción.",
                        "Para obtener un permiso de construcción, debe presentar un expediente completo que incluya una solicitud, planos de construcción, un pliego descriptivo y el certificado del terreno. El ayuntamiento tiene dos meses para responder. En ausencia de respuesta dentro de este plazo, el permiso se considera concedido."
                    ],
                    pt: [
                        "Antes de construir em um terreno no Benin, você deve obter: 1) Um certificado de urbanismo, 2) Uma licença de construção emitida pela prefeitura, 3) Uma autorização de ocupação do solo se aplicável. Os documentos a fornecer incluem o plano de localização, o plano do local e os planos arquitetônicos.",
                        "A construção em um terreno requer várias autorizações prévias. A licença de construção é obrigatória para qualquer construção nova e deve ser obtida antes do início dos trabalhos. O não cumprimento desta obrigação pode levar a sanções e demolição da construção.",
                        "Para obter uma licença de construção, você deve enviar um processo completo incluindo um requerimento, planos de construção, um memorial descritivo e o certificado do terreno. A prefeitura tem dois meses para responder. Na ausência de resposta dentro deste prazo, a licença é considerada concedida."
                    ]
                }
            },
            "vente": {
                patterns: ["vendre", "vente", "vendre terrain", "vendre propriété", "sell", "sale", "sell land", "sell property", "vender", "venta", "vender terreno", "vender propiedad", "vender", "venda", "vender terreno", "vender propriedade"],
                responses: {
                    fr: [
                        "Pour vendre un terrain au Bénin, le propriétaire doit : 1) Justifier de sa propriété par un titre foncier, 2) Établir un certificat de non-opposition, 3) Signer un acte de vente devant notaire, 4) Procéder à la mutation au service des domaines. Il est recommandé de faire appel à un notaire pour sécuriser la transaction.",
                        "La vente d'un terrain implique plusieurs étapes : verification des documents de propriété, établissement d'un compromis de vente, signature de l'acte authentique chez le notaire, et enregistrement de la mutation. Le notaire joue un rôle crucial dans la sécurisation juridique de l'opération.",
                        "Pour vendre en toute sécurité, il faut s'assurer de la validité du titre de propriété, obtenir un certificat de non-opposition, et faire authentifier l'acte de vente par un notaire. La publication au livre foncier consacre définitivement le transfert de propriété à l'acquéreur."
                    ],
                    en: [
                        "To sell land in Benin, the owner must: 1) Prove ownership with a land title, 2) Establish a non-opposition certificate, 3) Sign a deed of sale before a notary, 4) Proceed with the transfer at the domains service. It is recommended to use a notary to secure the transaction.",
                        "Selling land involves several steps: verification of ownership documents, establishment of a preliminary sales agreement, signing of the authentic deed with the notary, and registration of the transfer. The notary plays a crucial role in the legal security of the operation.",
                        "To sell safely, you must ensure the validity of the property title, obtain a non-opposition certificate, and have the deed of sale authenticated by a notary. Publication in the land register definitively consecrates the transfer of ownership to the buyer."
                    ],
                    es: [
                        "Para vender un terreno en Benin, el propietario debe: 1) Justificar su propiedad con un título de propiedad, 2) Establecer un certificado de no oposición, 3) Firmar una escritura de venta ante notario, 4) Proceder a la transferencia en el servicio de dominios. Se recomienda utilizar un notario para asegurar la transacción.",
                        "La venta de un terreno implica varios pasos: verificación de los documentos de propiedad, establecimiento de un acuerdo preliminar de venta, firma de la escritura auténtica con el notario y registro de la transferencia. El notario juega un papel crucial en la seguridad jurídica de la operación.",
                        "Para vender con seguridad, debe asegurarse de la validez del título de propiedad, obtener un certificado de no oposición y hacer autenticar la escritura de venta por un notario. La publicación en el registro de la propiedad consagra definitivamente la transferencia de propiedad al comprador."
                    ],
                    pt: [
                        "Para vender um terreno no Benin, o proprietário deve: 1) Comprovar a propriedade com um título de propriedade, 2) Estabelecer um certificado de não oposição, 3) Assinar uma escritura de venda perante notário, 4) Proceder à transferência no serviço de domínios. Recomenda-se usar um notário para garantir a transação.",
                        "A venda de um terreno envolve várias etapas: verificação dos documentos de propriedade, estabelecimento de um acordo preliminar de venda, assinatura da escritura autêntica com o notário e registro da transferência. O notário desempenha um papel crucial na segurança jurídica da operação.",
                        "Para vender com segurança, você deve garantir a validade do título de propriedade, obter um certificado de não oposição e fazer autenticar a escritura de venda por um notário. A publicação no registro de propriedade consagra definitivamente a transferência de propriedade para o comprador."
                    ]
                }
            }
        };

        // ===== ÉTAT DE L'APPLICATION =====
        let currentLanguage = 'fr';
        let conversationHistory = [];
        let responseCounters = {};

        // ===== FONCTIONS UTILITAIRES =====
        function findResponse(userInput, lang) {
            const input = userInput.toLowerCase();
            
            for (const [key, value] of Object.entries(knowledgeBase)) {
                for (const pattern of value.patterns) {
                    if (input.includes(pattern)) {
                        // Initialiser le compteur si nécessaire
                        if (!responseCounters[key]) {
                            responseCounters[key] = 0;
                        } else {
                            responseCounters[key]++;
                        }
                        
                        const responses = value.responses[lang];
                        const responseIndex = responseCounters[key] % responses.length;
                        return responses[responseIndex];
                    }
                }
            }
            
            // Réponses par défaut si aucun motif n'est reconnu
            const defaultResponses = {
                fr: [
                    "Je suis spécialisé dans les questions foncières au Bénin. Pourriez-vous reformuler votre question en rapport avec les terrains, propriétés ou droits fonciers ?",
                    "En tant qu'assistant foncier, je peux vous aider avec les questions liées aux acquisitions, litiges, documents et procédures foncières au Bénin. Pouvez-vous préciser votre demande ?",
                    "Je ne suis pas certain de comprendre votre question. Pourriez-vous la formuler autrement en mentionnant un aspect spécifique du droit foncier béninois ?",
                    "Désolé, mon expertise se limite au domaine foncier au Bénin. Posez-moi une question sur les titres de propriété, les litiges, les transactions ou la réglementation foncière."
                ],
                en: [
                    "I specialize in land issues in Benin. Could you rephrase your question regarding land, properties or land rights?",
                    "As a land assistant, I can help you with questions related to acquisitions, disputes, documents and land procedures in Benin. Can you specify your request?",
                    "I'm not sure I understand your question. Could you phrase it differently by mentioning a specific aspect of Beninese land law?",
                    "Sorry, my expertise is limited to the land domain in Benin. Ask me a question about property titles, disputes, transactions or land regulations."
                ],
                es: [
                    "Me especializo en cuestiones de tierras en Benin. ¿Podría reformular su pregunta con respecto a terrenos, propiedades o derechos de tierra?",
                    "Como asistente de tierras, puedo ayudarle con preguntas relacionadas con adquisiciones, disputas, documentos y procedimientos de tierra en Benin. ¿Puede especificar su solicitud?",
                    "No estoy seguro de entender su pregunta. ¿Podría formularla de otra manera mencionando un aspecto específico del derecho de tierras de Benin?",
                    "Lo siento, mi experiencia se limita al dominio de tierras en Benin. Hágame una pregunta sobre títulos de propiedad, disputas, transacciones o regulaciones de tierras."
                ],
                pt: [
                    "Sou especializado em questões de terra no Benin. Você poderia reformular sua pergunta sobre terrenos, propriedades ou direitos fundiários?",
                    "Como assistente fundiário, posso ajudá-lo com questões relacionadas a aquisições, disputas, documentos e procedimentos de terra no Benin. Você pode especificar seu pedido?",
                    "Não tenho certeza se entendi sua pergunta. Você poderia formulá-la de outra maneira mencionando um aspecto específico do direito fundiário beninense?",
                    "Desculpe, minha expertise é limitada ao domínio de terras no Benin. Faça-me uma pergunta sobre títulos de propriedade, disputas, transações ou regulamentações fundiárias."
                ]
            };
            
            const responses = defaultResponses[lang];
            return responses[Math.floor(Math.random() * responses.length)];
        }

        function simulateTyping() {
            const typingElement = document.createElement('div');
            typingElement.classList.add('message-typing');
            typingElement.innerHTML = `
                <div class="typing-dots">
                    <div class="typing-dot"></div>
                    <div class="typing-dot"></div>
                    <div class="typing-dot"></div>
                </div>
            `;
            document.getElementById('chatMessages').appendChild(typingElement);
            typingElement.scrollIntoView({ behavior: 'smooth' });
            return typingElement;
        }

        function removeTyping(typingElement) {
            if (typingElement && typingElement.parentNode) {
                typingElement.parentNode.removeChild(typingElement);
            }
        }

        function addMessage(message, isUser = false) {
            const messageElement = document.createElement('div');
            messageElement.classList.add('message');
            messageElement.classList.add(isUser ? 'user-message' : 'bot-message');
            messageElement.textContent = message;
            document.getElementById('chatMessages').appendChild(messageElement);
            messageElement.scrollIntoView({ behavior: 'smooth' });
            
            // Ajouter à l'historique
            if (isUser) {
                conversationHistory.push({ type: 'user', content: message });
            } else {
                conversationHistory.push({ type: 'bot', content: message });
            }
        }

        function processMessage() {
            const userInput = document.getElementById('userInput').value.trim();
            if (userInput === '') return;
            
            // Ajouter le message de l'utilisateur
            addMessage(userInput, true);
            
            // Effacer le champ de saisie
            document.getElementById('userInput').value = '';
            document.getElementById('userInput').style.height = 'auto';
            
            // Simuler le traitement
            const typingElement = simulateTyping();
            
            // Générer une réponse après un délai aléatoire (simulation de traitement)
            setTimeout(() => {
                removeTyping(typingElement);
                const response = findResponse(userInput, currentLanguage);
                addMessage(response, false);
            }, 1000 + Math.random() * 1000);
        }

        function changeLanguage(lang) {
            currentLanguage = lang;
            
            // Mettre à jour l'interface
            document.getElementById('currentLanguage').textContent = translations[lang].currentLanguage;
            document.getElementById('historyTitle').textContent = translations[lang].historyTitle;
            document.getElementById('history1').textContent = translations[lang].history1;
            document.getElementById('history2').textContent = translations[lang].history2;
            document.getElementById('history3').textContent = translations[lang].history3;
            document.getElementById('newChatText').textContent = translations[lang].newChatText;
            document.getElementById('chatbotTitle').textContent = translations[lang].chatbotTitle;
            document.getElementById('chatbotSubtitle').textContent = translations[lang].chatbotSubtitle;
            document.getElementById('welcomeMessage').textContent = translations[lang].welcomeMessage;
            document.getElementById('userInput').placeholder = translations[lang].inputPlaceholder;
            document.getElementById('suggestion1').textContent = translations[lang].suggestion1;
            document.getElementById('suggestion2').textContent = translations[lang].suggestion2;
            document.getElementById('suggestion3').textContent = translations[lang].suggestion3;
            document.getElementById('suggestion4').textContent = translations[lang].suggestion4;
            document.getElementById('aboutTitle').textContent = translations[lang].aboutTitle;
            document.getElementById('aboutText').textContent = translations[lang].aboutText;
            document.getElementById('linksTitle').textContent = translations[lang].linksTitle;
            document.getElementById('contactTitle').textContent = translations[lang].contactTitle;
            document.getElementById('rightsText').textContent = translations[lang].rightsText;
            
            // Mettre à jour les indicateurs de langue active
            document.querySelectorAll('.language-option i').forEach(icon => {
                icon.style.display = 'none';
            });
            document.getElementById(`check${lang.toUpperCase()}`).style.display = 'inline-block';
            
            // Cacher le dropdown
            document.getElementById('languageDropdown').classList.remove('show');
        }

        // ===== GESTION DES ÉVÉNEMENTS =====
        document.getElementById('sendBtn').addEventListener('click', processMessage);
        
        document.getElementById('userInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                processMessage();
            }
        });
        
        document.querySelectorAll('.suggestion-chip').forEach(chip => {
            chip.addEventListener('click', function() {
                const questionType = this.getAttribute('data-question');
                const questions = {
                    fr: {
                        documents: "Quels documents faut-il pour enregistrer un terrain ?",
                        litige: "Comment résoudre un litige foncier ?",
                        acquisition: "Quelle est la procédure d'acquisition d'un terrain ?",
                        titre: "Comment vérifier l'authenticité d'un titre foncier ?"
                    },
                    en: {
                        documents: "What documents are needed to register land?",
                        litige: "How to resolve a land dispute?",
                        acquisition: "What is the land acquisition procedure?",
                        titre: "How to verify the authenticity of a land title?"
                    },
                    es: {
                        documents: "¿Qué documentos se necesitan para registrar un terreno?",
                        litige: "¿Cómo resolver una disputa de tierra?",
                        acquisition: "¿Cuál es el procedimiento de adquisición de tierras?",
                        titre: "¿Cómo verificar la autenticidad de un título de propiedad?"
                    },
                    pt: {
                        documents: "Quais documentos são necessários para registrar um terreno?",
                        litige: "Como resolver uma disputa de terra?",
                        acquisition: "Qual é o procedimento de aquisição de terras?",
                        titre: "Como verificar a autenticidade de um título de propriedade?"
                    }
                };
                
                document.getElementById('userInput').value = questions[currentLanguage][questionType];
                processMessage();
            });
        });
        
        document.getElementById('newChatBtn').addEventListener('click', function() {
            const chatMessages = document.getElementById('chatMessages');
            while (chatMessages.children.length > 1) {
                chatMessages.removeChild(chatMessages.lastChild);
            }
            
            conversationHistory = [];
            responseCounters = {};
            
            addMessage(translations[currentLanguage].welcomeMessage, false);
        });

        document.getElementById('languageSelector').addEventListener('click', function(e) {
            e.stopPropagation();
            document.getElementById('languageDropdown').classList.toggle('show');
        });
        
        document.querySelectorAll('.language-option').forEach(option => {
            option.addEventListener('click', function() {
                const lang = this.getAttribute('data-lang');
                changeLanguage(lang);
            });
        });
        
        document.addEventListener('click', function() {
            document.getElementById('languageDropdown').classList.remove('show');
        });

        // Gestion du menu mobile
        const menuBtn = document.getElementById('menuBtn');
        const navLinks = document.getElementById('navLinks');
        
        menuBtn.addEventListener('click', () => {
            navLinks.classList.toggle('active');
            menuBtn.innerHTML = navLinks.classList.contains('active') ? 
                '<i class="fas fa-times"></i>' : '<i class="fas fa-bars"></i>';
        });

        // Ajustement automatique de la hauteur du textarea
        const textarea = document.getElementById('userInput');
        textarea.addEventListener('input', function() {
            this.style.height = 'auto';
            this.style.height = (this.scrollHeight) + 'px';
        });

        // Empêcher la fermeture du dropdown quand on clique dessus
        document.getElementById('languageDropdown').addEventListener('click', function(e) {
            e.stopPropagation();
        });
    </script>
</body>
</html>
