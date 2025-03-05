<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="icon" href="https://media.discordapp.net/attachments/1342153390104379402/1346628784668807349/a71b9f479ab2ed5d856314eb03079568.jpg?ex=67c8e149&is=67c78fc9&hm=694838b9586c303f2862d733c5967cef9875764d392825236177f54291e29570&=" sizes="128x128">
    <title>Clã Turquia - MushMC</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; background-color: #1b1b1b; color: white; margin: 0; padding: 0; }
        header { background: #f1c40f; padding: 20px; font-size: 24px; font-weight: bold; }
        .container { max-width: 800px; margin: auto; padding: 20px; }
        .card { background: #2c2c2c; padding: 20px; border-radius: 10px; margin-bottom: 20px; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); }
        .members { list-style: none; padding: 0; }
        .members li { 
            background: #444; 
            margin: 5px; 
            padding: 15px; 
            border-radius: 10px; 
            font-size: 18px; 
            display: flex; 
            justify-content: space-between; 
            align-items: center; 
            position: relative;
        }
        .members li .role {
            padding: 5px 15px;
            border-radius: 20px;
            font-weight: bold;
            text-transform: uppercase;
        }
        .button { padding: 18px 30px; background: #f1c40f; border: none; cursor: pointer; font-size: 18px; font-weight: bold; border-radius: 5px; transition: 0.3s; }
        .button:hover { background: #e1b800; }
        .remove-btn { background: red; border: none; color: white; padding: 8px 15px; cursor: pointer; border-radius: 5px; font-size: 16px; }
        .remove-btn:hover { background: darkred; }
        .rules, .contact { text-align: left; }
        a { color: #f1c40f; text-decoration: none; font-weight: bold; }
        a:hover { text-decoration: underline; }
        input[type="text"], select {
            padding: 15px;
            font-size: 18px;
            width: 100%;
            max-width: 400px;
            margin-bottom: 15px;
            border-radius: 5px;
            border: 1px solid #ccc;
            background-color: #333;
            color: white;
        }

        /* Cores específicas para os cargos */
        .role.Membro { background-color: #34495e; color: white; }
        .role.Recrutador { background-color: #2ecc71; color: white; }
        .role.Suporte { background-color: #3498db; color: white; }
        .role.Administrador { background-color: #e74c3c; color: white; }
        .role.Gerente { background-color: #f39c12; color: white; }
        .role.Sub-Líder { background-color: #9b59b6; color: white; }
        .role.Líder { background-color: #e67e22; color: white; }

        /* Créditos */
        .credits { font-size: 16px; color: #f1c40f; margin-top: 30px; text-align: center; }
        .founders { font-size: 18px; color: #f1c40f; margin-top: 30px; text-align: center; }
    </style>
</head>
<body>
    <header>Clã Turquia - MushMC</header>
    <div class="container">
        <div class="card">
            <h2>Sobre o Clã</h2>
            <p>Somos um time competitivo de BedWars no MushMC, sempre buscando a vitória e jogadas épicas!</p>
        </div>

        <div class="card">
            <h2>Membros</h2>
            <ul class="members" id="memberList"></ul>
        </div>

        <div class="card">
            <h2>Adicionar Membro</h2>
            <form id="addMemberForm">
                <input type="text" id="memberName" placeholder="Nome do membro" required>
                <select id="roleSelect">
                    <option value="">Selecione o Cargo</option>
                    <option value="Líder">Líder</option>
                    <option value="Sub-Líder">Sub Líder</option>
                    <option value="Gerente">Gerente</option>
                    <option value="Administrador">Administrador</option>
                    <option value="Recrutador">Recrutador</option>
                    <option value="Membro">Membro</option>
                </select>
                <button type="submit" class="button">Adicionar Membro</button>
            </form>
        </div>

        <div class="card rules">
            <h2>Regras do Clã</h2>
            <ul>
                <li>⚔️ Respeitar todos os membros.</li>
                <li>🔥 Ser ativo no servidor.</li>
                <li>🗣️ Manter uma boa comunicação no Discord.</li>
            </ul>
        </div>

        <div class="card contact">
            <h2>Contato</h2>
            <p>📢 Junte-se ao nosso Discord: <a href="https://discord.gg/Xt34QsdpgV" target="_blank">Discord do Clã Turquia</a></p>
        </div>

        <div class="founders">
            <h2>Founders</h2>
            <p>👑 sheikzadayt</p>
        </div>
    </div>

    <!-- Créditos -->
    <div class="credits">
        <p>Desenvolvido por: ChatGPT - OpenAI</p>
        <p>Baseado em código customizado e orientações de programação.</p>
    </div>

    <script>
        let members = []
        const memberList = document.getElementById('memberList');
        const addMemberForm = document.getElementById('addMemberForm');
        const memberNameInput = document.getElementById('memberName');
        const roleSelect = document.getElementById('roleSelect');

        function renderMembers() {
            memberList.innerHTML = '';
            members = JSON.parse(localStorage.getItem("members")) || []
            members.forEach((member, index) => {
                const li = document.createElement('li');
                const roleSpan = document.createElement('span');
                roleSpan.classList.add('role', member.role.replace(/\s+/g, '-')); // Adiciona a classe com a cor do cargo
                roleSpan.textContent = member.role;
                
                li.textContent = `${member.name} `;
                li.appendChild(roleSpan);

                const removeBtn = document.createElement('button');
                removeBtn.textContent = '❌';
                removeBtn.classList.add('remove-btn');
                removeBtn.onclick = () => removeMember(index);
                
                li.appendChild(removeBtn);
                memberList.appendChild(li);
            });
        }

        function addMember(event) {
            event.preventDefault();
            const newMemberName = memberNameInput.value;
            const selectedRole = roleSelect.value;

            if (!newMemberName || !selectedRole) {
                alert("Por favor, preencha o nome e o cargo do membro.");
                return;
            }

            members = JSON.parse(localStorage.getItem("members")) || [];
            members.push({ name: newMemberName, role: selectedRole });
            localStorage.setItem("members", JSON.stringify(members))
            
            memberNameInput.value = '';
            roleSelect.value = '';
            renderMembers();
        }

        function removeMember(index) {
            if (confirm(`Deseja remover ${members[index].name} do clã?`)) {

                members.splice(index, 1);
                console.log(members);

               
                localStorage.setItem("members", JSON.stringify(members))

                renderMembers();
            }
        }

        addMemberForm.addEventListener('submit', addMember);

        renderMembers();
    </script>
</body>
</html>
