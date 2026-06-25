<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <title>Zzy - Detektif Pembunuhan</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            user-select: none;
        }
        body {
            background: #0b0b16;
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            width: 100vw;
            overflow: hidden;
            touch-action: manipulation;
        }
        #app {
            width: 100%;
            max-width: 480px;
            height: 100vh;
            max-height: 860px;
            background: #111122;
            display: flex;
            flex-direction: column;
            box-shadow: 0 0 60px rgba(0, 0, 0, 0.9);
            position: relative;
            overflow: hidden;
        }
        /* Header */
        #header {
            background: #18182a;
            padding: 14px 18px;
            border-bottom: 2px solid #b89a6a;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-shrink: 0;
        }
        #header h1 {
            color: #b89a6a;
            font-size: 20px;
            font-weight: 700;
            letter-spacing: 0.5px;
        }
        #header h1 span {
            color: #b0b0c8;
            font-weight: 300;
        }
        #header-right {
            display: flex;
            align-items: center;
            gap: 12px;
        }
        #clue-badge {
            background: rgba(184, 154, 106, 0.12);
            border: 1px solid rgba(184, 154, 106, 0.15);
            border-radius: 20px;
            padding: 4px 14px;
            color: #b89a6a;
            font-size: 13px;
            font-weight: 600;
        }
        #reset-btn {
            background: transparent;
            border: none;
            color: #6a6a8a;
            font-size: 20px;
            cursor: pointer;
            touch-action: manipulation;
            padding: 0 4px;
            transition: color 0.15s;
        }
        #reset-btn:active {
            color: #b89a6a;
        }

        /* Main chat area */
        #chat {
            flex: 1;
            overflow-y: auto;
            padding: 16px 14px 10px;
            display: flex;
            flex-direction: column;
            gap: 8px;
            scroll-behavior: smooth;
            background: #0e0e1e;
        }
        .message {
            padding: 10px 14px;
            border-radius: 14px;
            max-width: 92%;
            word-wrap: break-word;
            line-height: 1.6;
            font-size: 15px;
            animation: msgIn 0.2s ease;
        }
        .message.narrator {
            background: #1a1a30;
            color: #c8c8dc;
            align-self: flex-start;
            border-bottom-left-radius: 4px;
            border: 1px solid #2a2a44;
        }
        .message.speaker {
            background: #2a2a4a;
            color: #e8e0d0;
            align-self: flex-start;
            border-bottom-left-radius: 4px;
            border-left: 3px solid #b89a6a;
        }
        .message.system {
            background: #1a2a1a;
            color: #a0c0a0;
            align-self: center;
            font-style: italic;
            font-size: 14px;
            border: 1px solid #2a4a2a;
            border-radius: 30px;
            padding: 6px 18px;
            max-width: 80%;
            text-align: center;
        }
        .message .who {
            font-weight: 700;
            color: #b89a6a;
            margin-right: 6px;
        }
        .message .who.zzy {
            color: #7ab8d8;
        }
        @keyframes msgIn {
            0% {
                opacity: 0;
                transform: translateY(8px);
            }
            100% {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* Choices area */
        #choices {
            padding: 10px 14px 14px;
            background: #0e0e1e;
            border-top: 1px solid #1a1a2e;
            flex-shrink: 0;
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            justify-content: center;
            max-height: 200px;
            overflow-y: auto;
        }
        .choice-btn {
            background: #1a1a32;
            border: 1px solid #2a2a4a;
            border-radius: 30px;
            padding: 10px 18px;
            color: #d0d0e0;
            font-size: 14px;
            font-weight: 500;
            cursor: pointer;
            touch-action: manipulation;
            transition: all 0.12s;
            flex: 1 0 auto;
            min-width: 80px;
            text-align: center;
            letter-spacing: 0.3px;
        }
        .choice-btn:active {
            background: #2a2a4a;
            border-color: #b89a6a;
            transform: scale(0.96);
        }
        .choice-btn.primary {
            background: #b89a6a;
            border-color: #b89a6a;
            color: #111122;
            font-weight: 700;
        }
        .choice-btn.primary:active {
            background: #a88858;
        }
        .choice-btn:disabled {
            opacity: 0.4;
            pointer-events: none;
        }

        /* Inventory bar */
        #inventory-bar {
            background: #0b0b18;
            padding: 6px 14px;
            border-top: 1px solid #18182a;
            display: flex;
            align-items: center;
            gap: 8px;
            flex-shrink: 0;
            min-height: 34px;
            overflow-x: auto;
            flex-wrap: nowrap;
        }
        #inventory-bar span.label {
            color: #5a6a7a;
            font-size: 11px;
            font-weight: 600;
            letter-spacing: 0.5px;
            white-space: nowrap;
        }
        .inv-chip {
            background: #1a1a2e;
            border: 1px solid #2a2a44;
            border-radius: 16px;
            padding: 2px 12px;
            font-size: 12px;
            color: #b0b0c8;
            white-space: nowrap;
            height: 26px;
            display: flex;
            align-items: center;
        }
        #inv-empty {
            color: #3a4a5a;
            font-size: 12px;
            font-style: italic;
        }

        /* Scrollbar */
        ::-webkit-scrollbar {
            width: 3px;
        }
        ::-webkit-scrollbar-track {
            background: transparent;
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(184, 154, 106, 0.15);
            border-radius: 10px;
        }

        /* Responsive */
        @media (max-width: 480px) {
            #header h1 {
                font-size: 17px;
            }
            #clue-badge {
                font-size: 12px;
                padding: 3px 10px;
            }
            .message {
                font-size: 14px;
                padding: 8px 12px;
            }
            .choice-btn {
                font-size: 13px;
                padding: 8px 14px;
                min-width: 60px;
            }
            #inventory-bar {
                padding: 4px 10px;
                min-height: 28px;
            }
            .inv-chip {
                font-size: 11px;
                height: 22px;
                padding: 0 10px;
            }
        }
        @media (max-height: 700px) {
            #chat {
                padding: 10px 12px 6px;
            }
            .message {
                font-size: 13px;
                padding: 6px 10px;
            }
            .choice-btn {
                font-size: 12px;
                padding: 6px 12px;
            }
            #header {
                padding: 10px 14px;
            }
            #header h1 {
                font-size: 16px;
            }
        }
    </style>
</head>
<body>
    <div id="app">
        <!-- Header -->
        <div id="header">
            <h1><span>Z</span>zy</h1>
            <div id="header-right">
                <div id="clue-badge">Bukti: <span id="clue-count">0</span>/4</div>
                <button id="reset-btn" aria-label="Reset">↻</button>
            </div>
        </div>

        <!-- Chat area -->
        <div id="chat"></div>

        <!-- Choices -->
        <div id="choices"></div>

        <!-- Inventory -->
        <div id="inventory-bar">
            <span class="label">INVENTARIS</span>
            <div id="inventory-items" style="display:flex;gap:4px;flex-wrap:nowrap;overflow-x:auto;flex:1;padding:2px 0;">
                <span id="inv-empty">(kosong)</span>
            </div>
        </div>
    </div>

    <script>
        // =============================================================
        //  GAME DATA
        // =============================================================
        const G = {
            location: 'livingroom',
            inventory: [],
            cluesCollected: 0,
            totalClues: 4,
            talkedTo: {},
            gameOver: false,
            killerId: 'mrs_blackwood',
            suspects: [
                { id: 'mrs_blackwood', name: 'Mrs. Blackwood', desc: 'Istri korban, motif warisan' },
                { id: 'mr_green', name: 'Mr. Green', desc: 'Butler, motif hutang' },
                { id: 'ms_white', name: 'Ms. White', desc: 'Sekretaris, motif pemerasan' },
            ],
            items: {
                letter: { id: 'letter', name: 'Surat Ancaman', desc: 'Surat ancaman dari Mrs. Blackwood untuk suaminya.' },
                knife: { id: 'knife', name: 'Pisau Berdarah', desc: 'Pisau dapur dengan noda darah.' },
                will: { id: 'will', name: 'Surat Wasiat', desc: 'Wasiat yang menyebut Mrs. Blackwood sebagai pewaris tunggal.' },
                recording: { id: 'recording', name: 'Rekaman Telepon', desc: 'Rekaman percakapan Mrs. Blackwood yang mengancam.' },
            },
            // Lokasi dan objek
            locations: {
                livingroom: {
                    name: 'Ruang Tamu',
                    desc: 'Anda berada di ruang tamu yang luas. Ada sofa, meja, dan beberapa pintu. Tersangka berkeliaran di sini.',
                    objects: [
                        { id: 'letter', type: 'item', label: 'Surat di atas meja', collectId: 'letter' },
                        { id: 'mrs_blackwood', type: 'npc', label: 'Mrs. Blackwood (istri korban)', npcId: 'mrs_blackwood' },
                        { id: 'mr_green', type: 'npc', label: 'Mr. Green (butler)', npcId: 'mr_green' },
                        { id: 'door_kitchen', type: 'door', label: 'Pintu ke dapur', target: 'kitchen' },
                        { id: 'door_bedroom', type: 'door', label: 'Pintu ke kamar tidur', target: 'bedroom' },
                        { id: 'door_garden', type: 'door', label: 'Pintu ke taman', target: 'garden' },
                    ]
                },
                kitchen: {
                    name: 'Dapur',
                    desc: 'Dapur bersih dengan peralatan masak. Ada pisau di atas meja.',
                    objects: [
                        { id: 'knife', type: 'item', label: 'Pisau dapur', collectId: 'knife' },
                        { id: 'ms_white', type: 'npc', label: 'Ms. White (sekretaris)', npcId: 'ms_white' },
                        { id: 'door_living', type: 'door', label: 'Kembali ke ruang tamu', target: 'livingroom' },
                    ]
                },
                bedroom: {
                    name: 'Kamar Tidur',
                    desc: 'Kamar tidur utama. Ada lemari dan tempat tidur. Surat wasiat tergeletak di atas meja.',
                    objects: [
                        { id: 'will', type: 'item', label: 'Surat wasiat', collectId: 'will' },
                        { id: 'door_living2', type: 'door', label: 'Kembali ke ruang tamu', target: 'livingroom' },
                    ]
                },
                garden: {
                    name: 'Taman',
                    desc: 'Taman belakang yang rimbun. Ada bangku taman. Suasana tenang.',
                    objects: [
                        { id: 'recording', type: 'item', label: 'Rekaman telepon (dari Ms. White)', collectId: 'recording' },
                        { id: 'door_living3', type: 'door', label: 'Kembali ke ruang tamu', target: 'livingroom' },
                    ]
                }
            },
            // Dialog pohon
            dialogues: {
                mrs_blackwood: {
                    initial: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Saya tidak tahu apa-apa. Suami saya sudah tidak mencintai saya, tapi saya tidak membunuhnya.',
                        options: [
                            { label: 'Di mana anda tadi malam?', next: 'alibi' },
                            { label: 'Surat ancaman ini untuk anda?', next: 'letter_reaction' },
                            { label: 'Saya tahu anda punya motif.', next: 'motif' },
                        ]
                    },
                    alibi: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Saya di kamar tidur sepanjang malam. Sendirian. Tidak ada saksi.',
                        options: [
                            { label: 'Kembali', next: 'initial' },
                            { label: 'Wasiat menyebut nama anda.', next: 'will_reaction' },
                        ]
                    },
                    letter_reaction: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Itu hanya surat marah biasa. Saya tidak benar-benar bermaksud membunuhnya.',
                        options: [
                            { label: 'Jadi anda mengancamnya?', next: 'threat_admit' },
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    threat_admit: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Saya marah karena dia berselingkuh. Tapi saya tidak membunuhnya.',
                        options: [
                            { label: 'Saya punya rekaman telepon anda.', next: 'phone_reaction' },
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    phone_reaction: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Itu hanya panggilan biasa. Saya tidak tahu tentang rekaman itu.',
                        options: [
                            { label: 'Cukup. Saya tahu anda pelakunya.', next: 'confront' },
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    will_reaction: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Wasiat? Dia bilang dia akan mengubahnya. Tapi dia tidak sempat.',
                        options: [
                            { label: 'Jadi anda membunuhnya untuk warisan?', next: 'confront' },
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    motif: {
                        speaker: 'Mrs. Blackwood',
                        text: 'Saya tahu saya akan mendapat warisan, tapi itu bukan alasan untuk membunuh.',
                        options: [
                            { label: 'Saya akan cari bukti lebih.', next: 'initial' },
                            { label: 'Rekaman telepon anda berbicara.', next: 'phone_reaction' },
                        ]
                    },
                    confront: {
                        speaker: 'Mrs. Blackwood',
                        text: '...Anda benar. Saya membunuhnya. Dia akan meninggalkan saya tanpa apa-apa. Maafkan saya.',
                        options: [
                            { label: 'Saya menangkap anda.', next: 'arrest' },
                        ]
                    },
                    arrest: {
                        speaker: 'Zzy',
                        text: 'Mrs. Blackwood, anda ditangkap atas pembunuhan suami anda. Keadilan ditegakkan.',
                        options: [
                            { label: 'Kasus selesai.', next: 'end' },
                        ]
                    },
                    end: {
                        speaker: 'KASUS TERPECAHKAN',
                        text: 'Selamat! Zzy berhasil mengungkap pembunuhan Mr. Blackwood. Mrs. Blackwood adalah pelakunya.',
                        options: []
                    }
                },
                mr_green: {
                    initial: {
                        speaker: 'Mr. Green',
                        text: 'Saya hanya pelayan. Saya tidak tahu apa-apa tentang pembunuhan ini.',
                        options: [
                            { label: 'Di mana anda tadi malam?', next: 'alibi' },
                            { label: 'Pisau di dapur milik anda?', next: 'knife' },
                        ]
                    },
                    alibi: {
                        speaker: 'Mr. Green',
                        text: 'Saya di dapur sepanjang malam, membersihkan. Tidak ada saksi.',
                        options: [
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    knife: {
                        speaker: 'Mr. Green',
                        text: 'Pisau itu dari dapur, tapi saya tidak menggunakannya untuk membunuh.',
                        options: [
                            { label: 'Kembali', next: 'initial' },
                        ]
                    }
                },
                ms_white: {
                    initial: {
                        speaker: 'Ms. White',
                        text: 'Saya sekretaris Mr. Blackwood. Dia orang baik. Saya tidak percaya dia dibunuh.',
                        options: [
                            { label: 'Di mana anda tadi malam?', next: 'alibi' },
                            { label: 'Surat ancaman ini untuk anda?', next: 'letter' },
                            { label: 'Ada rekaman telepon mencurigakan?', next: 'recording_offer' },
                        ]
                    },
                    alibi: {
                        speaker: 'Ms. White',
                        text: 'Saya di ruang kerja sampai larut. Banyak pekerjaan.',
                        options: [
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    letter: {
                        speaker: 'Ms. White',
                        text: 'Surat itu? Saya tahu Mrs. Blackwood menulisnya. Dia sangat marah pada suaminya.',
                        options: [
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    recording_offer: {
                        speaker: 'Ms. White',
                        text: 'Ya, saya punya rekaman percakapan Mrs. Blackwood yang mengancam suaminya. Saya berikan pada anda.',
                        options: [
                            { label: 'Ambil rekaman', next: 'take_recording' },
                            { label: 'Kembali', next: 'initial' },
                        ]
                    },
                    take_recording: {
                        speaker: 'Zzy',
                        text: 'Rekaman telepon berhasil didapat. Ini bukti kuat.',
                        options: [
                            { label: 'Simpan', next: 'initial' },
                        ],
                        onEnter: function() {
                            if (!G.inventory.includes('recording')) {
                                G.inventory.push('recording');
                                G.cluesCollected++;
                                updateUI();
                                addMessage('system', 'Rekaman telepon ditambahkan ke inventaris.');
                            }
                        }
                    }
                }
            }
        };

        // =============================================================
        //  STATE
        // =============================================================
        let currentDialogue = null; // { npcId, nodeId }

        // =============================================================
        //  DOM REFS
        // =============================================================
        const chatEl = document.getElementById('chat');
        const choicesEl = document.getElementById('choices');
        const inventoryItems = document.getElementById('inventory-items');
        const clueCount = document.getElementById('clue-count');
        const resetBtn = document.getElementById('reset-btn');

        // =============================================================
        //  UTILITY
        // =============================================================
        function addMessage(type, content, who = '') {
            const div = document.createElement('div');
            div.className = 'message ' + type;
            if (type === 'speaker') {
                div.innerHTML = `<span class="who">${who}</span> ${content}`;
            } else if (type === 'narrator') {
                div.textContent = content;
            } else if (type === 'system') {
                div.textContent = content;
            }
            chatEl.appendChild(div);
            chatEl.scrollTop = chatEl.scrollHeight;
        }

        function clearChoices() {
            choicesEl.innerHTML = '';
        }

        function showChoices(options) {
            clearChoices();
            if (!options || options.length === 0) {
                const btn = document.createElement('button');
                btn.className = 'choice-btn primary';
                btn.textContent = 'Lanjutkan';
                btn.addEventListener('click', () => {
                    clearChoices();
                    addMessage('narrator', 'Ketuk objek atau orang untuk berinteraksi.');
                });
                btn.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    btn.click();
                });
                choicesEl.appendChild(btn);
                return;
            }
            options.forEach(opt => {
                const btn = document.createElement('button');
                btn.className = 'choice-btn';
                btn.textContent = opt.label;
                btn.addEventListener('click', () => {
                    if (opt.next && currentDialogue) {
                        const npcId = currentDialogue.npcId;
                        const nextNode = opt.next;
                        if (nextNode === 'end') {
                            G.gameOver = true;
                            addMessage('system', 'KASUS TERPECAHKAN! Zzy berhasil menangkap pembunuh.');
                            clearChoices();
                            updateUI();
                            return;
                        }
                        startDialogue(npcId, nextNode);
                    }
                });
                btn.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    btn.click();
                });
                choicesEl.appendChild(btn);
            });
        }

        // =============================================================
        //  CORE GAME FUNCTIONS
        // =============================================================
        function enterLocation(locationId) {
            G.location = locationId;
            const loc = G.locations[locationId];
            if (!loc) return;

            addMessage('narrator', '--- ' + loc.name + ' ---');
            addMessage('narrator', loc.desc);

            // Tampilkan objek sebagai pilihan
            const objOptions = loc.objects.map(obj => {
                if (obj.type === 'item') {
                    const already = G.inventory.includes(obj.collectId);
                    return {
                        label: (already ? '[✔] ' : '[ ] ') + obj.label,
                        action: () => {
                            if (already) {
                                addMessage('system', 'Anda sudah mengambil ini.');
                                return;
                            }
                            const item = G.items[obj.collectId];
                            if (item) {
                                G.inventory.push(obj.collectId);
                                G.cluesCollected++;
                                addMessage('narrator', 'Anda menemukan ' + item.name + '. ' + item.desc);
                                updateUI();
                                // Refresh scene after collect
                                enterLocation(G.location);
                            }
                        }
                    };
                } else if (obj.type === 'npc') {
                    return {
                        label: 'Bicara dengan ' + obj.label,
                        action: () => {
                            startDialogue(obj.npcId, 'initial');
                        }
                    };
                } else if (obj.type === 'door') {
                    return {
                        label: 'Masuk ke ' + obj.label,
                        action: () => {
                            enterLocation(obj.target);
                        }
                    };
                }
                return null;
            }).filter(Boolean);

            // Tambahkan opsi "Periksa Bukti" jika semua terkumpul
            if (G.cluesCollected >= G.totalClues && !G.gameOver) {
                objOptions.push({
                    label: '[⚖️] Tuduh Pelaku',
                    action: () => {
                        openAccuseModal();
                    }
                });
            }

            // Tampilkan pilihan
            clearChoices();
            objOptions.forEach(opt => {
                const btn = document.createElement('button');
                btn.className = 'choice-btn';
                btn.textContent = opt.label;
                btn.addEventListener('click', opt.action);
                btn.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    btn.click();
                });
                choicesEl.appendChild(btn);
            });

            // Jika tidak ada pilihan (misal akhir), tampilkan tombol reset
            if (objOptions.length === 0) {
                const btn = document.createElement('button');
                btn.className = 'choice-btn primary';
                btn.textContent = 'Kembali ke Ruang Tamu';
                btn.addEventListener('click', () => {
                    enterLocation('livingroom');
                });
                btn.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    btn.click();
                });
                choicesEl.appendChild(btn);
            }
        }

        function startDialogue(npcId, nodeId) {
            const tree = G.dialogues[npcId];
            if (!tree) return;
            const node = tree[nodeId];
            if (!node) return;

            G.talkedTo[npcId] = true;
            currentDialogue = { npcId, nodeId };

            if (node.onEnter) {
                node.onEnter();
                updateUI();
            }

            addMessage('speaker', node.text, node.speaker || '');
            const options = node.options || [];
            showChoices(options);

            // Jika tidak ada pilihan, setelah 3 detik kembali ke lokasi
            if (options.length === 0) {
                setTimeout(() => {
                    if (choicesEl.children.length === 1 && choicesEl.children[0].textContent === 'Lanjutkan') {
                        // Do nothing, user will click
                    }
                }, 100);
            }
        }

        // =============================================================
        //  ACCUSE MODAL (sederhana, berbasis chat)
        // =============================================================
        function openAccuseModal() {
            if (G.gameOver) return;
            if (G.cluesCollected < G.totalClues) {
                addMessage('system', 'Kumpulkan semua bukti terlebih dahulu!');
                return;
            }
            addMessage('narrator', '--- TUDUH PELAKU ---');
            addMessage('narrator', 'Pilih tersangka yang anda yakini sebagai pembunuh:');

            const suspectOptions = G.suspects.map(s => {
                return {
                    label: s.name,
                    action: () => {
                        makeAccusation(s.id);
                    }
                };
            });
            // Tambahkan opsi batal
            suspectOptions.push({
                label: 'Batal',
                action: () => {
                    addMessage('system', 'Anda membatalkan tuduhan.');
                    enterLocation(G.location);
                }
            });

            clearChoices();
            suspectOptions.forEach(opt => {
                const btn = document.createElement('button');
                btn.className = 'choice-btn';
                btn.textContent = opt.label;
                btn.addEventListener('click', opt.action);
                btn.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    btn.click();
                });
                choicesEl.appendChild(btn);
            });
        }

        function makeAccusation(suspectId) {
            if (suspectId === G.killerId) {
                addMessage('system', '✅ BENAR! ' + G.suspects.find(s => s.id === suspectId).name + ' adalah pembunuhnya.');
                addMessage('narrator', 'Motif: ' + G.suspects.find(s => s.id === suspectId).desc);
                addMessage('system', '🏆 Zzy berhasil memecahkan kasus!');
                G.gameOver = true;
                updateUI();
                clearChoices();
                const btn = document.createElement('button');
                btn.className = 'choice-btn primary';
                btn.textContent = 'Mulai Ulang';
                btn.addEventListener('click', resetGame);
                btn.addEventListener('touchend', (e) => {
                    e.preventDefault();
                    btn.click();
                });
                choicesEl.appendChild(btn);
            } else {
                const suspect = G.suspects.find(s => s.id === suspectId);
                addMessage('system', '❌ SALAH! ' + suspect.name + ' bukan pembunuhnya.');
                addMessage('narrator', 'Periksa kembali bukti dan coba lagi.');
                // Kembali ke lokasi setelah beberapa saat
                setTimeout(() => {
                    enterLocation(G.location);
                }, 1500);
            }
        }

        // =============================================================
        //  UI UPDATE
        // =============================================================
        function updateUI() {
            // Inventory
            inventoryItems.innerHTML = '';
            if (G.inventory.length === 0) {
                inventoryItems.innerHTML = '<span id="inv-empty">(kosong)</span>';
            } else {
                G.inventory.forEach(id => {
                    const item = G.items[id];
                    if (!item) return;
                    const chip = document.createElement('span');
                    chip.className = 'inv-chip';
                    chip.textContent = item.name;
                    inventoryItems.appendChild(chip);
                });
            }
            clueCount.textContent = G.cluesCollected;
        }

        // =============================================================
        //  RESET
        // =============================================================
        function resetGame() {
            G.location = 'livingroom';
            G.inventory = [];
            G.cluesCollected = 0;
            G.talkedTo = {};
            G.gameOver = false;
            currentDialogue = null;
            chatEl.innerHTML = '';
            clearChoices();
            updateUI();
            // Tampilkan intro
            addMessage('narrator', '--- KASUS PEMBUNUHAN MR. BLACKWOOD ---');
            addMessage('narrator', 'Selamat datang, detektif Zzy. Anda ditugaskan untuk menyelidiki pembunuhan Mr. Blackwood.');
            addMessage('narrator', 'Kumpulkan bukti, wawancarai tersangka, dan temukan pembunuhnya.');
            addMessage('system', 'Petunjuk: ketuk objek atau orang untuk berinteraksi.');
            enterLocation('livingroom');
        }

        // =============================================================
        //  INIT
        // =============================================================
        resetBtn.addEventListener('click', resetGame);
        resetBtn.addEventListener('touchend', (e) => {
            e.preventDefault();
            resetGame();
        });

        // Mulai game
        resetGame();

        console.log('Zzy - Detektif Pembunuhan (teks)');
        console.log('Kasus: Pembunuhan Mr. Blackwood');
        console.log('Tersangka: Mrs. Blackwood, Mr. Green, Ms. White');
        console.log('Bukti: Surat Ancaman, Pisau Berdarah, Surat Wasiat, Rekaman Telepon');
        console.log('Pembunuh: Mrs. Blackwood');
    </script>
</body>
</html>
