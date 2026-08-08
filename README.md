/**
 * ============================================
 * ZURY CHAT — Aplicação Principal
 * ============================================
 * Arquitetura: Classe unica com módulos internos
 * Padrão: async/await, separação de responsabilidades,
 *         event delegation, cache de estado.
 * ============================================
 */

class ZuryApp {
    // ===== Estado centralizado =====
    state = {
        currentUser: null,
        currentChat: null,
        currentChatUser: null,
        replyToMessage: null,
        recordingTimer: null,
        recordingSeconds: 0,
        messagesUnsub: null,
        conversationsUnsub: null,
        darkMode: false,
        notificationsEnabled: true,
        themeColor: '#1a73e8',
        aiMessages: [],
        mediaRecorder: null,
        recordedChunks: []
    };

    // ===== Catálogo de emojis =====
    emojis = {
        recent: ['👍','❤️','😂','😮','😢','🙏','🔥','👏','🎉','😍','🤔','👌','😊','🤣','😭','🥰','😎','🤩','😘','🤗'],
        smileys: ['😀','😃','😄','😁','😆','😅','🤣','😂','🙂','🙃','😉','😊','😇','🥰','😍','🤩','😘','😗','☺️','😚','😙','🥲','😋','😛','😜','🤪','😝','🤑','🤗','🤭','🤫','🤔','🤐','🤨','😐','😑','😶','😏','😒','🙄','😬','🤥','😌','😔','😪','🤤','😴','😷','🤒','🤕','🤢','🤮','🤧','🥵','🥶','🥴','😵','🤯','🤠','🥳','🥸','😎','🤓','🧐','😕','😟','🙁','☹️','😮','😯','😲','😳','🥺','😦','😧','😨','😰','😥','😢','😭','😱','😖','😣','😞','😓','😩','😫','🥱','😤','😡','😠','🤬','😈','👿','💀','☠️','💩','🤡','👹','👺','👻','👽','👾','🤖','😺','😸','😹','😻','😼','😽','🙀','😿','😾','❤️','🧡','💛','💚','💙','💜','🖤','🤍','🤎','💔','❣️','💕','💞','💓','💗','💖','💘','💝'],
        animals: ['🐶','🐱','🐭','🐹','🐰','🦊','🐻','🐼','🐨','🐯','🦁','🐮','🐷','🐽','🐸','🐵','🙈','🙉','🙊','🐒','🐔','🐧','🐦','🐤','🐣','🐥','🦆','🦅','🦉','🦇','🐺','🐗','🐴','🦄','🐝','🐛','🦋','🐌','🐞','🐜','🦟','🦗','🕷️','🕸️','🦂','🐢','🐍','🦎','🦖','🦕','🐙','🦑','🦐','🦞','🦀','🐡','🐠','🐟','🐬','🐳','🐋','🦈','🐊','🐅','🐆','🦓','🦍','🦧','🐘','🦛','🦏','🐪','🐫','🦒','🦘','🐃','🐂','🐄','🐎','🐖','🐏','🐑','🦙','🐐','🦌','🐕','🐩','🦮','🐕‍🦺','🐈','🐈‍⬛','🐓','🦃','🦚','🦜','🦢','🦩','🕊️','🐇','🦝','🦨','🦡','🦦','🦥','🐁','🐀','🐿️','🦔'],
        food: ['🍏','🍎','🍐','🍊','🍋','🍌','🍉','🍇','🍓','🫐','🍈','🍒','🍑','🍍','🥝','🥥','🥑','🍆','🥔','🥕','🌽','🌶️','🫑','🥒','🥬','🥦','🧄','🧅','🍄','🥜','🌰','🍞','🥐','🥖','🥨','🥯','🥞','🧇','🧀','🍖','🍗','🥩','🥓','🍔','🍟','🍕','🌭','🥪','🌮','🌯','🫔','🥙','🧆','🥚','🍳','🥘','🍲','🫕','🥣','🥗','🍿','🧈','🧂','🥫','🍱','🍘','🍙','🍚','🍛','🍜','🍝','🍠','🍢','🍣','🍤','🍥','🍡','🍦','🍧','🍨','🍩','🍪','🎂','🍰','🧁','🥧','🍫','🍬','🍭','🍮','🍯','🍼','🥛','☕','🫖','🍵','🍶','🍾','🍷','🍸','🍹','🍺','🍻','🥂','🥃','🥤','🧋','🧃','🧉','🧊'],
        activities: ['⚽','🏀','🏈','⚾','🥎','🎾','🏐','🏉','🥏','🎱','🪀','🏓','🏸','🏒','🏑','🥍','🏏','🪃','🥅','⛳','🪁','🏹','🎣','🤿','🥊','🥋','🎽','🛹','🛼','🛷','⛸️','🥌','🎿','⛷️','🏂','🪂','🏋️','🤼','🤽','🤾','🌋','⛰️','🏔️','🗻','🏕️','🏖️','🏜️','🏝️','🏞️','🏟️','🏛️','🏗️','🧱','🪨','🪵','🛖','🏘️','🏚️','🏠','🏡','🏢','🏣','🏤','🏥','🏦','🏨','🏩','🏪','🏫','🏬','🏭','🏯','🏰','💒','🗼','🗽','⛪','🕌','🛕','🕍','⛩️','🕋','⛲','⛺','🌁','🌃','🏙️','🌄','🌅','🌆','🌇','🌉','♨️','🎠','🎡','🎢','💈','🎪']
    };

    // ===== Dicionário de erros do Firebase Auth =====
    authErrors = {
        'auth/invalid-email': 'E-mail inválido',
        'auth/user-not-found': 'Usuário não encontrado',
        'auth/wrong-password': 'Senha incorreta',
        'auth/email-already-in-use': 'E-mail já cadastrado',
        'auth/weak-password': 'Senha deve ter pelo menos 6 caracteres',
        'auth/invalid-credential': 'E-mail ou senha incorretos',
        'auth/network-request-failed': 'Erro de conexão. Verifique sua internet',
        'auth/too-many-requests': 'Muitas tentativas. Tente novamente mais tarde',
        'auth/user-disabled': 'Esta conta foi desativada'
    };

    // ===== Mapeamento de telas =====
    viewMap = {
        main: 'conversations-view',
        search: 'search-view',
        profile: 'profile-view',
        'edit-profile': 'edit-profile-view',
        settings: 'settings-view',
        files: 'files-view',
        'ai-assistant': 'ai-assistant-view'
    };

    // ===== Construtor =====
    constructor() {
        this.bindGlobalEvents();
    }

    // ============================================
    // INICIALIZAÇÃO
    // ============================================
    init() {
        this.loadSettings();
        this.initAuthListener();
        this.animateSplashScreen();
    }

    animateSplashScreen() {
        setTimeout(() => {
            document.querySelector('.loading-progress').style.width = '100%';
        }, 100);
        setTimeout(() => {
            document.getElementById('splash-screen').classList.remove('active');
            document.getElementById('auth-screen').classList.add('active');
        }, 2200);
    }

    // ============================================
    // CONFIGURAÇÕES / TEMA
    // ============================================
    loadSettings() {
        try {
            const raw = localStorage.getItem('zurySettings');
            if (!raw) return;

            const settings = JSON.parse(raw);
            this.state.darkMode = settings.darkMode || false;
            this.state.themeColor = settings.themeColor || '#1a73e8';
            this.state.notificationsEnabled = settings.notifications !== false;

            if (this.state.darkMode) {
                document.documentElement.setAttribute('data-theme', 'dark');
                this.getEl('dark-mode-toggle').checked = true;
            }
            this.setThemeColor(this.state.themeColor);
            this.getEl('notif-toggle').checked = this.state.notificationsEnabled;
        } catch {
            /* silencioso */ }
    }

    saveSettings() {
        const payload = {
            darkMode: this.state.darkMode,
            themeColor: this.state.themeColor,
            notifications: this.state.notificationsEnabled
        };
        localStorage.setItem('zurySettings', JSON.stringify(payload));
    }

    toggleDarkMode() {
        this.state.darkMode = this.getEl('dark-mode-toggle').checked;
        document.documentElement.setAttribute(
            'data-theme',
            this.state.darkMode ? 'dark' : 'light'
        );
        this.saveSettings();
    }

    setThemeColor(color) {
        this.state.themeColor = color;
        document.documentElement.style.setProperty('--primary', color);

        const r = parseInt(color.slice(1, 3), 16);
        const g = parseInt(color.slice(3, 5), 16);
        const b = parseInt(color.slice(5, 7), 16);
        const darker = `rgb(${Math.floor(r * 0.7)}, ${Math.floor(g * 0.7)}, ${Math.floor(b * 0.7)})`;
        document.documentElement.style.setProperty('--primary-dark', darker);

        document.querySelectorAll('.color-option').forEach(opt => {
            const isActive = opt.style.background === color || opt.getAttribute('onclick')?.includes(color);
            opt.classList.toggle('active', isActive);
        });

        this.saveSettings();
    }

    toggleNotifications() {
        this.state.notificationsEnabled = this.getEl('notif-toggle').checked;
        this.saveSettings();
        this.showToast(this.state.notificationsEnabled ? 'Notificações ativadas' : 'Notificações desativadas');
    }

    // ============================================
    // AUTENTICAÇÃO
    // ============================================
    initAuthListener() {
        auth.onAuthStateChanged(user => {
            if (user) {
                this.state.currentUser = user;
                this.loadUserData(user.uid);
                this.showScreen('main');
            } else {
                this.state.currentUser = null;
                this.showAuthForm('login');
            }
        });
    }

    showAuthForm(form) {
        document.querySelectorAll('.auth-form').forEach(f => f.classList.remove('active'));
        this.getEl(`${form}-form`).classList.add('active');
    }

    togglePassword(id) {
        const input = this.getEl(id);
        input.type = input.type === 'password' ? 'text' : 'password';
    }

    // --- Login com email/senha ---
    async login() {
        const email = this.getEl('login-email').value.trim();
        const password = this.getEl('login-password').value;

        if (!email || !password) {
            return this.showToast('Preencha todos os campos');
        }

        try {
            await auth.signInWithEmailAndPassword(email, password);
            this.showToast('Login realizado com sucesso!');
        } catch (err) {
            this.showToast(this.authErrors[err.code] || 'Erro ao autenticar');
        }
    }

    // --- Cadastro ---
    async register() {
        const name = this.getEl('reg-name').value.trim();
        const username = this.getEl('reg-username').value.trim();
        const email = this.getEl('reg-email').value.trim();
        const password = this.getEl('reg-password').value;

        if (!name || !username || !email || !password) {
            return this.showToast('Preencha todos os campos');
        }
        if (password.length < 6) {
            return this.showToast('A senha deve ter pelo menos 6 caracteres');
        }

        try {
            const cred = await auth.createUserWithEmailAndPassword(email, password);
            await usersRef.doc(cred.user.uid).set({
                uid: cred.user.uid,
                name,
                username: username.toLowerCase(),
                email,
                bio: '',
                photoURL: '',
                createdAt: firebase.firestore.FieldValue.serverTimestamp(),
                lastSeen: firebase.firestore.FieldValue.serverTimestamp(),
                online: true
            });
            this.showToast('Conta criada com sucesso!');
        } catch (err) {
            this.showToast(this.authErrors[err.code] || 'Erro ao criar conta');
        }
    }

    // --- Recuperar senha ---
    async resetPassword() {
        const email = this.getEl('forgot-email').value.trim();
        if (!email) return this.showToast('Digite seu e-mail');

        try {
            await auth.sendPasswordResetEmail(email);
            this.showToast('Link de recuperação enviado!');
            this.showAuthForm('login');
        } catch (err) {
            this.showToast(this.authErrors[err.code] || 'Erro ao enviar e-mail');
        }
    }

    // --- Login com Google ---
    async loginWithGoogle() {
        try {
            const provider = new firebase.auth.GoogleAuthProvider();
            const result = await auth.signInWithPopup(provider);
            const user = result.user;

            const userDoc = await usersRef.doc(user.uid).get();
            if (!userDoc.exists) {
                await usersRef.doc(user.uid).set({
                    uid: user.uid,
                    name: user.displayName || 'Usuário',
                    username: (user.email?.split('@')[0] || 'user').toLowerCase(),
                    email: user.email,
                    bio: '',
                    photoURL: user.photoURL || '',
                    createdAt: firebase.firestore.FieldValue.serverTimestamp(),
                    lastSeen: firebase.firestore.FieldValue.serverTimestamp(),
                    online: true
                });
            }
            this.showToast('Login com Google realizado!');
        } catch {
            this.showToast('Erro no login com Google');
        }
    }

    // --- Logout ---
    async logout() {
        if (this.state.currentUser) {
            await usersRef.doc(this.state.currentUser.uid).update({
                online: false,
                lastSeen: firebase.firestore.FieldValue.serverTimestamp()
            });
        }
        await auth.signOut();
        this.showToast('Sessão encerrada');
        this.showAuthForm('login');
    }

    // ============================================
    // DADOS DO USUÁRIO
    // ============================================
    async loadUserData(uid) {
        try {
            const doc = await usersRef.doc(uid).get();
            if (!doc.exists) return;

            const data = doc.data();
            this.updateProfileUI(data);
            this.updateOnlineStatus(true);
            this.loadConversations();
        } catch (err) {
            console.error('Erro ao carregar usuário:', err);
        }
    }

    updateProfileUI(data) {
        const photo = data.photoURL || 'assets/default-avatar.png';
        this.getEl('header-avatar').src = photo;
        this.getEl('profile-name').textContent = data.name || 'Usuário';
        this.getEl('profile-username').textContent = '@' + (data.username || 'user');
        this.getEl('profile-bio').textContent = data.bio || 'Nenhuma bio';
        this.getEl('profile-photo').src = photo;
    }

    async updateOnlineStatus(online) {
        if (!this.state.currentUser) return;
        await usersRef.doc(this.state.currentUser.uid).update({
            online,
            lastSeen: firebase.firestore.FieldValue.serverTimestamp()
        });
    }

    // ============================================
    // NAVEGAÇÃO / UI
    // ============================================
    showScreen(screen) {
        document.querySelectorAll('#main-app .view').forEach(v => v.classList.remove('active'));
        this.getEl('header-menu')?.classList.remove('show');

        const viewId = this.viewMap[screen];
        if (viewId) this.getEl(viewId).classList.add('active');

        const handlers = {
            main: () => this.loadConversations(),
            files: () => this.loadFiles(),
            'ai-assistant': () => this.loadAIMessages()
        };
        handlers[screen]?.();
    }

    toggleMenu() {
        this.getEl('header-menu').classList.toggle('show');
    }

    // ============================================
    // CONVERSAS
    // ============================================
    loadConversations() {
        if (this.state.conversationsUnsub) this.state.conversationsUnsub();
        if (!this.state.currentUser) return;

        this.state.conversationsUnsub = conversationsRef
            .where('participants', 'array-contains', this.state.currentUser.uid)
            .orderBy('lastMessageTime', 'desc')
            .onSnapshot(snapshot => {
                const list = this.getEl('conversations-list');
                list.innerHTML = '';

                if (snapshot.empty) {
                    list.innerHTML = this.emptyStateHTML('comments', 'Nenhuma conversa ainda', 'Busque um usuário para começar');
                    return;
                }

                snapshot.forEach(doc => {
                    const conv = doc.data();
                    const otherUid = conv.participants.find(p => p !== this.state.currentUser.uid);
                    this.renderConversationItem(doc.id, conv, otherUid);
                });
            }, err => console.error('Listener de conversas:', err));
    }

    async renderConversationItem(convId, conv, otherUid) {
        try {
            const userDoc = await usersRef.doc(otherUid).get();
            const user = userDoc.exists ? userDoc.data() : { name: 'Usuário', photoURL: '', online: false };

            const time = conv.lastMessageTime ? this.formatTime(conv.lastMessageTime.toDate()) : '';
            const unread = conv.unreadCount?.[this.state.currentUser.uid] || 0;
            const isOnline = user.online ? 'online' : '';

            const item = document.createElement('div');
            item.className = 'conversation-item';
            item.innerHTML = `
                <img src="${user.photoURL || 'assets/default-avatar.png'}" class="conv-avatar ${isOnline}">
                <div class="conv-info">
                    <div class="conv-name">${this.escapeHtml(user.name || 'Usuário')}</div>
                    <div class="conv-last-msg">${this.escapeHtml(conv.lastMessage || 'Inicie uma conversa')}</div>
                </div>
                <div class="conv-meta">
                    <div class="conv-time">${time}</div>
                    ${unread > 0 ? `<span class="unread-badge">${unread}</span>` : ''}
                </div>
            `;
            item.onclick = () => this.openChat(convId, otherUid, user);
            this.getEl('conversations-list').appendChild(item);
        } catch (err) {
            console.error('Erro ao renderizar conversa:', err);
        }
    }

    searchConversations(query) {
        const q = query.toLowerCase();
        document.querySelectorAll('.conversation-item').forEach(item => {
            const name = item.querySelector('.conv-name').textContent.toLowerCase();
            item.style.display = name.includes(q) ? 'flex' : 'none';
        });
    }

    emptyStateHTML(icon, title, subtitle) {
        return `
            <div style="text-align:center;padding:60px 20px;color:var(--text-muted)">
                <i class="fas fa-${icon}" style="font-size:48px;margin-bottom:16px;opacity:0.5"></i>
                <p>${title}</p>
                ${subtitle ? `<p style="font-size:14px;margin-top:8px">${subtitle}</p>` : ''}
            </div>
        `;
    }

    // ============================================
    // BUSCA DE USUÁRIOS
    // ============================================
    #searchDebounce;
    searchUsers(query) {
        clearTimeout(this.#searchDebounce);
        const results = this.getEl('search-results');

        if (!query.trim()) {
            results.innerHTML = '';
            return;
        }

        this.#searchDebounce = setTimeout(async () => {
            try {
                const snapshot = await usersRef
                    .orderBy('name')
                    .startAt(query)
                    .endAt(query + '\uf8ff')
                    .limit(20)
                    .get();

                results.innerHTML = '';
                snapshot.forEach(doc => {
                    const user = doc.data();
                    if (user.uid === this.state.currentUser?.uid) return;

                    const div = document.createElement('div');
                    div.className = 'user-result';
                    div.innerHTML = `
                        <img src="${user.photoURL || 'assets/default-avatar.png'}" alt="${user.name}">
                        <div class="user-result-info">
                            <div class="user-result-name">${this.escapeHtml(user.name)}</div>
                            <div class="user-result-username">@${user.username}</div>
                        </div>
                    `;
                    div.onclick = () => this.startChat(doc.id, user);
                    results.appendChild(div);
                });
            } catch (err) {
                console.error('Erro na busca:', err);
            }
        }, 300);
    }

    // ============================================
    // CHAT — Iniciar / Abrir
    // ============================================
    async startChat(otherUid, userData) {
        try {
            const [q1, q2] = await Promise.all([
                conversationsRef.where('participants', '==', [this.state.currentUser.uid, otherUid]).limit(1).get(),
                conversationsRef.where('participants', '==', [otherUid, this.state.currentUser.uid]).limit(1).get()
            ]);

            let convId;
            if (!q1.empty) convId = q1.docs[0].id;
            else if (!q2.empty) convId = q2.docs[0].id;
            else {
                const newConv = await conversationsRef.add({
                    participants: [this.state.currentUser.uid, otherUid],
                    createdAt: firebase.firestore.FieldValue.serverTimestamp(),
                    lastMessageTime: firebase.firestore.FieldValue.serverTimestamp(),
                    lastMessage: '',
                    unreadCount: { [this.state.currentUser.uid]: 0, [otherUid]: 0 }
                });
                convId = newConv.id;
            }

            this.openChat(convId, otherUid, userData);
        } catch (err) {
            console.error('Erro ao iniciar conversa:', err);
            this.showToast('Erro ao iniciar conversa');
        }
    }

    openChat(convId, otherUid, userData) {
        this.state.currentChat = convId;
        this.state.currentChatUser = { uid: otherUid, ...userData };

        this.getEl('chat-name').textContent = userData.name || 'Usuário';
        this.getEl('chat-avatar').src = userData.photoURL || 'assets/default-avatar.png';
        this.getEl('chat-status').textContent = userData.online ? 'online' : 'offline';
        this.getEl('chat-status').className = 'status-text ' + (userData.online ? 'online' : '');

        this.getEl('chat-view').classList.add('active');
        this.state.replyToMessage = null;
        this.getEl('reply-bar').classList.remove('show');

        this.loadMessages(convId);
    }

    closeChat() {
        if (this.state.messagesUnsub) {
            this.state.messagesUnsub();
            this.state.messagesUnsub = null;
        }
        this.state.currentChat = null;
        this.state.currentChatUser = null;
        this.state.replyToMessage = null;
        this.getEl('reply-bar').classList.remove('show');
        this.getEl('chat-view').classList.remove('active');
    }

    // ============================================
    // CHAT — Mensagens
    // ============================================
    loadMessages(convId) {
        if (this.state.messagesUnsub) this.state.messagesUnsub();

        const container = this.getEl('chat-messages');
        container.innerHTML = '<div style="text-align:center;padding:40px;color:var(--text-muted)"><i class="fas fa-spinner fa-spin"></i></div>';

        this.state.messagesUnsub = messagesRef
            .where('conversationId', '==', convId)
            .orderBy('timestamp', 'asc')
            .limitToLast(100)
            .onSnapshot(snapshot => {
                container.innerHTML = '';
                snapshot.forEach(doc => this.renderMessage(doc.id, doc.data()));
                this.scrollToBottom();

                // Marcar como lida
                this.markMessagesAsRead(convId);
            }, err => console.error('Listener de mensagens:', err));
    }

    renderMessage(id, msg) {
        const container = this.getEl('chat-messages');
        const isMe = msg.senderId === this.state.currentUser?.uid;
        const div = document.createElement('div');
        div.className = `message ${isMe ? 'sent' : 'received'}`;
        div.id = `msg-${id}`;

        const time = msg.timestamp ? this.formatTime(msg.timestamp.toDate()) : '';
        let content = '';

        if (msg.replyTo) {
            content += `
                <div class="reply-preview">
                    <span class="reply-sender">${this.escapeHtml(msg.replyTo.name || 'Usuário')}</span>
                    <span class="reply-text-preview">${this.escapeHtml(msg.replyTo.text || 'Mídia')}</span>
                </div>
            `;
        }

        switch (msg.type) {
            case 'image':
                content += `<img src="${msg.mediaUrl}" class="message-media" onclick="app.previewMedia('${msg.mediaUrl}','image')">`;
                break;
            case 'video':
                content += `<video src="${msg.mediaUrl}" class="message-media" controls onclick="app.previewMedia('${msg.mediaUrl}','video')"></video>`;
                break;
            case 'audio':
                content += `<audio src="${msg.mediaUrl}" controls class="message-audio"></audio>`;
                break;
            case 'file':
                content += `<a href="${msg.mediaUrl}" target="_blank" class="message-file"><i class="fas fa-file"></i> ${this.escapeHtml(msg.fileName || 'Arquivo')}</a>`;
                break;
            default:
                content += `<div class="message-text">${this.escapeHtml(msg.text)}</div>`;
        }

        div.innerHTML = `
            ${content}
            <div class="message-meta">
                <span class="message-time">${time}</span>
                ${isMe ? `<span class="message-status"><i class="fas fa-${msg.read ? 'check-double' : 'check'}"></i></span>` : ''}
            </div>
        `;

        // Toque longo para responder
        let pressTimer;
        const startPress = () => { pressTimer = setTimeout(() => this.setReplyMessage(id, msg), 600); };
        const cancelPress = () => clearTimeout(pressTimer);

        div.addEventListener('touchstart', startPress, { passive: true });
        div.addEventListener('touchend', cancelPress);
        div.addEventListener('mousedown', startPress);
        div.addEventListener('mouseup', cancelPress);

        container.appendChild(div);
    }

    async markMessagesAsRead(convId) {
        try {
            const batch = db.batch();
            const snapshot = await messagesRef
                .where('conversationId', '==', convId)
                .where('senderId', '!=', this.state.currentUser.uid)
                .where('read', '==', false)
                .get();

            snapshot.forEach(doc => batch.update(doc.ref, { read: true }));
            await batch.commit();
        } catch (err) {
            console.error('Erro ao marcar como lida:', err);
        }
    }

    // ============================================
    // CHAT — Enviar mensagens
    // ============================================
    async sendMessage() {
        const input = this.getEl('message-input');
        const text = input.value.trim();
        if (!text || !this.state.currentChat) return;

        const payload = {
            conversationId: this.state.currentChat,
            senderId: this.state.currentUser.uid,
            text,
            type: 'text',
            timestamp: firebase.firestore.FieldValue.serverTimestamp(),
            read: false
        };

        if (this.state.replyToMessage) {
            payload.replyTo = {
                id: this.state.replyToMessage.id,
                name: this.state.replyToMessage.senderName,
                text: this.state.replyToMessage.text
            };
            this.cancelReply();
        }

        try {
            await messagesRef.add(payload);
            await conversationsRef.doc(this.state.currentChat).update({
                lastMessage: text,
                lastMessageTime: firebase.firestore.FieldValue.serverTimestamp(),
                [`unreadCount.${this.state.currentChatUser.uid}`]: firebase.firestore.FieldValue.increment(1)
            });
            input.value = '';
        } catch (err) {
            console.error('Erro ao enviar mensagem:', err);
            this.showToast('Erro ao enviar mensagem');
        }
    }

    handleMessageKey(e) {
        if (e.key === 'Enter') this.sendMessage();
    }

    setReplyMessage(id, msg) {
        this.state.replyToMessage = {
            id,
            senderName: msg.senderId === this.state.currentUser?.uid ? 'Você' : this.state.currentChatUser?.name || 'Usuário',
            text: msg.text || 'Mídia'
        };
        this.getEl('reply-name').textContent = this.state.replyToMessage.senderName;
        this.getEl('reply-text').textContent = this.state.replyToMessage.text;
        this.getEl('reply-bar').classList.add('show');
        this.getEl('message-input').focus();
    }

    cancelReply() {
        this.state.replyToMessage = null;
        this.getEl('reply-bar').classList.remove('show');
    }

    // ============================================
    // CHAT — Anexos / Mídia
    // ============================================
    showAttachmentMenu() {
        this.getEl('attachment-menu').classList.toggle('show');
        this.getEl('emoji-picker').classList.remove('show');
    }

    openCamera() {
        this.getEl('camera-input').click();
        this.closeMenus();
    }

    selectPhoto() {
        this.getEl('photo-input').accept = 'image/*';
        this.getEl('photo-input').click();
        this.closeMenus();
    }

    selectVideo() {
        this.getEl('photo-input').accept = 'video/*';
        this.getEl('photo-input').click();
        this.closeMenus();
    }

    selectFile() {
        this.getEl('file-input').accept = '*/*';
        this.getEl('file-input').click();
        this.closeMenus();
    }

    selectAudio() {
        this.getEl('file-input').accept = 'audio/*';
        this.getEl('file-input').click();
        this.closeMenus();
    }

    closeMenus() {
        this.getEl('attachment-menu').classList.remove('show');
    }

    async handleFileSelect(e) {
        const file = e.target.files[0];
        if (!file || !this.state.currentChat) return;

        const typeMap = {
            'image/': 'image',
            'video/': 'video',
            'audio/': 'audio'
        };
        let type = 'file';
        for (const [prefix, mapped] of Object.entries(typeMap)) {
            if (file.type.startsWith(prefix)) { type = mapped; break; }
        }

        try {
            this.showToast('Enviando arquivo...');
            const path = `chats/${this.state.currentChat}/${Date.now()}_${file.name}`;
            const ref = storage.ref(path);
            await ref.put(file);
            const url = await ref.getDownloadURL();

            await messagesRef.add({
                conversationId: this.state.currentChat,
                senderId: this.state.currentUser.uid,
                type,
                mediaUrl: url,
                fileName: file.name,
                fileSize: file.size,
                text: '',
                timestamp: firebase.firestore.FieldValue.serverTimestamp(),
                read: false
            });

            const labelMap = { image: '📷 Foto', video: '🎥 Vídeo', audio: '🎵 Áudio' };
            await conversationsRef.doc(this.state.currentChat).update({
                lastMessage: labelMap[type] || '📎 Arquivo',
                lastMessageTime: firebase.firestore.FieldValue.serverTimestamp(),
                [`unreadCount.${this.state.currentChatUser.uid}`]: firebase.firestore.FieldValue.increment(1)
            });

            this.showToast('Arquivo enviado!');
        } catch (err) {
            console.error('Erro ao enviar arquivo:', err);
            this.showToast('Erro ao enviar arquivo');
        }

        e.target.value = '';
    }

    // ============================================
    // CHAT — Emoji Picker
    // ============================================
    toggleEmojiPicker() {
        this.getEl('emoji-picker').classList.toggle('show');
        this.closeMenus();
    }

    showEmojiCategory(category) {
        document.querySelectorAll('.emoji-tab').forEach(t => t.classList.remove('active'));
        event?.target?.classList.add('active');

        const grid = this.getEl('emoji-grid');
        grid.innerHTML = '';
        (this.emojis[category] || []).forEach(emoji => {
            const span = document.createElement('span');
            span.textContent = emoji;
            span.onclick = () => this.insertEmoji(emoji);
            grid.appendChild(span);
        });
    }

    insertEmoji(emoji) {
        const input = this.getEl('message-input');
        input.value += emoji;
        input.focus();
    }

    // ============================================
    // CHAT — Gravação de áudio
    // ============================================
    async startRecording() {
        if (!navigator.mediaDevices) {
            this.showToast('Gravação não suportada neste dispositivo');
            return;
        }
        try {
            const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
            this.state.mediaRecorder = new MediaRecorder(stream);
            this.state.recordedChunks = [];

            this.state.mediaRecorder.ondataavailable = e => {
                if (e.data.size > 0) this.state.recordedChunks.push(e.data);
            };

            this.state.mediaRecorder.onstop = () => this.processRecording();

            this.state.mediaRecorder.start();
            this.state.recordingSeconds = 0;
            this.getEl('recording-ui').classList.add('show');
            this.getEl('record-btn').classList.add('recording');

            this.state.recordingTimer = setInterval(() => {
                this.state.recordingSeconds++;
                const m = Math.floor(this.state.recordingSeconds / 60).toString().padStart(2, '0');
                const s = (this.state.recordingSeconds % 60).toString().padStart(2, '0');
                document.querySelector('.recording-time').textContent = `${m}:${s}`;
            }, 1000);
        } catch {
            this.showToast('Permita o acesso ao microfone');
        }
    }

    stopRecording() {
        if (!this.state.mediaRecorder || this.state.mediaRecorder.state === 'inactive') return;
        this.state.mediaRecorder.stop();
        this.state.mediaRecorder.stream.getTracks().forEach(t => t.stop());
        clearInterval(this.state.recordingTimer);
        this.getEl('recording-ui').classList.remove('show');
        this.getEl('record-btn').classList.remove('recording');
    }

    async processRecording() {
        if (!this.state.currentChat || this.state.recordedChunks.length === 0) return;
        const blob = new Blob(this.state.recordedChunks, { type: 'audio/webm' });

        try {
            const path = `chats/${this.state.currentChat}/audio_${Date.now()}.webm`;
            const ref = storage.ref(path);
            await ref.put(blob);
            const url = await ref.getDownloadURL();

            const m = Math.floor(this.state.recordingSeconds / 60);
            const s = (this.state.recordingSeconds % 60).toString().padStart(2, '0');

            await messagesRef.add({
                conversationId: this.state.currentChat,
                senderId: this.state.currentUser.uid,
                type: 'audio',
                mediaUrl: url,
                duration: `${m}:${s}`,
                text: '',
                timestamp: firebase.firestore.FieldValue.serverTimestamp(),
                read: false
            });

            await conversationsRef.doc(this.state.currentChat).update({
                lastMessage: '🎵 Áudio',
                lastMessageTime: firebase.firestore.FieldValue.serverTimestamp(),
                [`unreadCount.${this.state.currentChatUser.uid}`]: firebase.firestore.FieldValue.increment(1)
            });
        } catch (err) {
            console.error('Erro ao enviar áudio:', err);
            this.showToast('Erro ao enviar áudio');
        }
    }

    previewMedia(url, type) {
        // Implementação futura: lightbox/modal para preview
        window.open(url, '_blank');
    }

    // ============================================
    // PERFIL
    // ============================================
    editProfile() {
        this.getEl('edit-name').value = this.getEl('profile-name').textContent;
        this.getEl('edit-username').value = this.getEl('profile-username').textContent.replace('@', '');
        const bio = this.getEl('profile-bio').textContent;
        this.getEl('edit-bio').value = bio === 'Nenhuma bio' ? '' : bio;
        this.getEl('edit-profile-photo').src = this.getEl('profile-photo').src;
        this.showScreen('edit-profile');
    }

    async saveProfile() {
        const name = this.getEl('edit-name').value.trim();
        const username = this.getEl('edit-username').value.trim();
        const bio = this.getEl('edit-bio').value.trim();

        if (!name || !username) return this.showToast('Nome e usuário são obrigatórios');

        try {
            await usersRef.doc(this.state.currentUser.uid).update({ name, username: username.toLowerCase(), bio });
            this.getEl('profile-name').textContent = name;
            this.getEl('profile-username').textContent = '@' + username;
            this.getEl('profile-bio').textContent = bio || 'Nenhuma bio';
            this.showToast('Perfil atualizado!');
            this.showScreen('profile');
        } catch {
            this.showToast('Erro ao salvar perfil');
        }
    }

    changeProfilePhoto() {
        this.getEl('profile-photo-input').click();
    }

    async handleProfilePhoto(e) {
        const file = e.target.files[0];
        if (!file) return;

        try {
            this.showToast('Enviando foto...');
            const path = `users/${this.state.currentUser.uid}/profile.jpg`;
            const ref = storage.ref(path);
            await ref.put(file);
            const url = await ref.getDownloadURL();

            await usersRef.doc(this.state.currentUser.uid).update({ photoURL: url });
            this.getEl('profile-photo').src = url;
            this.getEl('header-avatar').src = url;
            this.getEl('edit-profile-photo').src = url;
            this.showToast('Foto atualizada!');
        } catch {
            this.showToast('Erro ao atualizar foto');
        }
        e.target.value = '';
    }

    showUserProfile() {
        if (!this.state.currentChatUser) return;
        this.showToast(`Perfil de ${this.state.currentChatUser.name || 'Usuário'}`);
    }

    showChatMenu() {
        this.showToast('Menu do chat (em breve)');
    }

    // ============================================
    // ARQUIVOS / MÍDIA
    // ============================================
    showFileCategory(cat) {
        document.querySelectorAll('.file-tab').forEach(t => t.classList.remove('active'));
        event?.target?.classList.add('active');
        this.loadFiles(cat);
    }

    async loadFiles(category = 'all') {
        const grid = this.getEl('files-grid');
        grid.innerHTML = '<div style="grid-column:1/-1;text-align:center;padding:40px;color:var(--text-muted)"><i class="fas fa-spinner fa-spin" style="font-size:24px"></i></div>';

        try {
            const snapshot = await messagesRef
                .where('senderId', '==', this.state.currentUser.uid)
                .where('type', 'in', ['image', 'video', 'audio', 'file'])
                .orderBy('timestamp', 'desc')
                .limit(50)
                .get();

            grid.innerHTML = '';
            snapshot.forEach(doc => {
                const msg = doc.data();
                if (!this.filterFileCategory(msg.type, category)) return;

                const div = document.createElement('div');
                div.className = 'file-item';
                div.innerHTML = this.buildFileItemHTML(msg);
                div.onclick = () => msg.mediaUrl && window.open(msg.mediaUrl, '_blank');
                grid.appendChild(div);
            });

            if (!grid.hasChildNodes()) {
                grid.innerHTML = this.emptyStateHTML('folder-open', 'Nenhum arquivo encontrado');
            }
        } catch (err) {
            console.error('Erro ao carregar arquivos:', err);
            grid.innerHTML = '<div style="grid-column:1/-1;text-align:center;padding:40px;color:var(--text-muted)">Erro ao carregar arquivos</div>';
        }
    }

    filterFileCategory(type, category) {
        if (category === 'all') return true;
        const map = { photos: 'image', videos: 'video', audios: 'audio', docs: 'file' };
        return type === map[category];
    }

    buildFileItemHTML(msg) {
        if (msg.type === 'image') return `<img src="${msg.mediaUrl}" loading="lazy">`;
        if (msg.type === 'video') return `<video src="${msg.mediaUrl}"></video><div class="file-overlay"><i class="fas fa-play"></i></div>`;
        if (msg.type === 'audio') return `<div style="display:flex;align-items:center;justify-content:center;height:100%"><i class="fas fa-music" style="font-size:32px;color:var(--primary)"></i></div>`;
        return `<div style="display:flex;align-items:center;justify-content:center;height:100%"><i class="fas fa-file-alt" style="font-size:32px;color:var(--text-muted)"></i></div>`;
    }

    // ============================================
    // ASSISTENTE IA
    // ============================================
    loadAIMessages() {
        const container = this.getEl('ai-messages');
        container.innerHTML = '';

        if (this.state.aiMessages.length === 0) {
            this.addAIMessage('bot', 'Olá! Sou o Assistente Zury AI. Posso ajudar você a:\n\n• Corrigir textos\n• Traduzir mensagens\n• Resumir conversas\n• Sugerir respostas\n\nComo posso ajudar?');
        } else {
            this.state.aiMessages.forEach(m => this.addAIMessage(m.role, m.text, false));
        }
    }

    addAIMessage(role, text, save = true) {
        const container = this.getEl('ai-messages');
        const div = document.createElement('div');
        div.className = `ai-message ${role}`;

        const escaped = this.escapeHtml(text).replace(/\n/g, '<br>');
        div.innerHTML = role === 'bot'
            ? `<i class="fas fa-robot"></i> ${escaped}`
            : escaped;

        container.appendChild(div);
        container.scrollTop = container.scrollHeight;

        if (save) {
            this.state.aiMessages.push({ role, text });
            if (this.state.aiMessages.length > 50) this.state.aiMessages.shift();
        }
    }

    handleAIKey(e) {
        if (e.key === 'Enter') this.sendAIMessage();
    }

    async sendAIMessage() {
        const input = this.getEl('ai-input');
        const text = input.value.trim();
        if (!text) return;

        this.addAIMessage('user', text);
        input.value = '';

        // Indicador de digitação
        const container = this.getEl('ai-messages');
        const typing = document.createElement('div');
        typing.className = 'typing-indicator show';
        typing.id = 'ai-typing';
        typing.innerHTML = '<span></span><span></span><span></span>';
        container.appendChild(typing);
        container.scrollTop = container.scrollHeight;

        // Simular resposta (substituir por chamada real a API de IA)
        await new Promise(r => setTimeout(r, 1200));
        this.getEl('ai-typing')?.remove();
        this.addAIMessage('bot', this.generateAIResponse(text));
    }

    askAI(action) {
        const prompts = {
            corrigir: 'Corrija o seguinte texto: ',
            traduzir: 'Traduza para português: ',
            resumir: 'Resuma o seguinte texto: ',
            sugerir: 'Sugira uma resposta para: '
        };
        this.getEl('ai-input').value = prompts[action] || '';
        this.getEl('ai-input').focus();
    }

    generateAIResponse(input) {
        const lower = input.toLowerCase();
        const actions = [
            { test: /corrigir|corrija/, label: 'Texto corrigido', extra: '✅ Correções aplicadas!' },
            { test: /traduzir|traduza/, label: 'Tradução', extra: '→ Tradução concluída.' },
            { test: /resumir|resuma/, label: 'Resumo', extra: '→ Resumo conciso do texto.' },
            { test: /sugerir|sugira/, label: 'Sugestões', extra: 'Escolha uma ou peça mais.' }
        ];

        for (const act of actions) {
            if (act.test.test(lower)) {
                const clean = input.replace(/corrigir|corrija|traduzir|traduza|resumir|resuma|sugerir|sugira/gi, '').trim();
                if (clean) return `${act.label}:\n\n"${clean}"\n\n${act.extra}`;
                return 'Envie o texto que deseja processar.';
            }
        }

        if (/oi|olá|ola/.test(lower)) {
            return 'Olá! Como posso ajudar? Posso corrigir, traduzir, resumir ou sugerir respostas.';
        }
        if (/ajuda|help/.test(lower)) {
            return 'Comandos:\n• "Corrija: [texto]"\n• "Traduza: [texto]"\n• "Resuma: [texto]"\n• "Sugira: [contexto]"';
        }

        return `Entendi: "${input}"\n\nComo posso ajudar? Use Corrigir, Traduzir, Resumir ou Sugerir.`;
    }

    // ============================================
    // NOTIFICAÇÕES
    // ============================================
    sendNotification(title, body) {
        if ('Notification' in window && Notification.permission === 'granted' && document.hidden) {
            new Notification(title, { body, icon: 'assets/zury-logo.png', badge: 'assets/zury-logo.png' });
        }
    }

    requestNotificationPermission() {
        if ('Notification' in window && Notification.permission === 'default') {
            Notification.requestPermission();
        }
    }

    // ============================================
    // UTILITÁRIOS
    // ============================================
    getEl(id) {
        return document.getElementById(id);
    }

    scrollToBottom() {
        const container = this.getEl('chat-messages');
        if (container) container.scrollTop = container.scrollHeight;
    }

    formatTime(date) {
        if (!date) return '';
        const now = new Date();
        const d = new Date(date);
        const diff = now - d;
        const days = Math.floor(diff / (1000 * 60 * 60 * 24));

        if (days === 0) return d.toLocaleTimeString('pt-BR', { hour: '2-digit', minute: '2-digit' });
        if (days === 1) return 'Ontem';
        if (days < 7) return d.toLocaleDateString('pt-BR', { weekday: 'short' });
        return d.toLocaleDateString('pt-BR', { day: '2-digit', month: '2-digit' });
    }

    formatFileSize(bytes) {
        if (!bytes) return '0 B';
        const k = 1024;
        const sizes = ['B', 'KB', 'MB', 'GB'];
        const i = Math.floor(Math.log(bytes) / Math.log(k));
        return parseFloat((bytes / Math.pow(k, i)).toFixed(1)) + ' ' + sizes[i];
    }

    escapeHtml(text) {
        if (!text) return '';
        const div = document.createElement('div');
        div.textContent = text;
        return div.innerHTML;
    }

    showToast(message) {
        const toast = this.getEl('toast');
        this.getEl('toast-message').textContent = message;
        toast.classList.add('show');
        setTimeout(() => toast.classList.remove('show'), 3000);
    }

    // ============================================
    // EVENTOS GLOBAIS
    // ============================================
    bindGlobalEvents() {
        document.addEventListener('visibilitychange', () => {
            this.updateOnlineStatus(!document.hidden);
        });

        document.addEventListener('click', e => {
            if (!e.target.closest('.header-right')) {
                this.getEl('header-menu')?.classList.remove('show');
            }
            if (!e.target.closest('.chat-input-area') && !e.target.closest('.attachment-menu') && !e.target.closest('.emoji-picker')) {
                this.getEl('attachment-menu')?.classList.remove('show');
                this.getEl('emoji-picker')?.classList.remove('show');
            }
        });

        window.addEventListener('resize', () => {
            if (this.state.currentChat) setTimeout(() => this.scrollToBottom(), 300);
        });
    }
}

// ===== Instancia única =====
const app = new ZuryApp();
document.addEventListener('DOMContentLoaded', () => app.init());
