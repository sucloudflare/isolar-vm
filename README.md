
  <h1>🛡️ Teste Seguro de Malware no VirtualBox (Rede NAT Isolada)</h1>
    <p>Este guia explica como configurar um <strong>ambiente isolado</strong> no VirtualBox para testar e analisar vírus/malwares, evitando que afetem seu computador principal ou a internet real.</p>

   <div class="warning">
        <h2>⚠️ Aviso de Segurança</h2>
        <ul>
            <li><strong>Não execute malware no seu sistema principal</strong>.</li>
            <li>Sempre use máquinas virtuais isoladas.</li>
            <li>Não permita que a rede da VM tenha acesso direto à internet real.</li>
            <li>Este guia é apenas para fins educacionais e de pesquisa.</li>
        </ul>
    </div>

  <h2>📋 Requisitos</h2>
    <ul>
        <li>VirtualBox instalado</li>
        <li><code>VBoxManage</code> disponível no terminal</li>
        <li>Uma VM com o malware para testes (<code>vmmmm</code> no exemplo)</li>
        <li>Uma VM Kali Linux para análise de tráfego</li>
        <li>Wireshark ou tcpdump</li>
    </ul>

   <h2>🔹 1. Criar rede NAT isolada</h2>
    <pre><code>VBoxManage natnetwork add --netname NatIsolado --network "10.10.0.0/24" --enable --dhcp on</code></pre>
    <p>Isso cria uma rede NAT isolada chamada <strong>NatIsolado</strong> com IPs na faixa <code>10.10.0.x</code>.</p>

   <h2>🔹 2. Conectar a VM infectada à rede isolada</h2>
    <p>Desligue a VM antes de aplicar:</p>
    <pre><code>VBoxManage controlvm "vmmmm" poweroff</code></pre>
    <p>Configure para usar a rede NAT isolada:</p>
    <pre><code>VBoxManage modifyvm "vmmmm" --nic1 natnetwork --nat-network1 "NatIsolado"</code></pre>

   <h2>🔹 3. Criar uma VM "Sniffer" para monitorar</h2>
    <p>Use uma segunda VM (ex.: Kali Sniffer) conectada na mesma rede <strong>NatIsolado</strong> para capturar tráfego.</p>
    <ul>
        <li>Placa de rede 1 → Tipo: Rede NAT</li>
        <li>Rede NAT → NatIsolado</li>
    </ul>

   <h2>🔹 4. Capturar tráfego com tcpdump</h2>
    <p>Na VM "Kali Sniffer":</p>
    <pre><code>sudo apt update && sudo apt install tcpdump -y</code></pre>
    <p>Identifique a interface:</p>
    <pre><code>ip a</code></pre>
    <p>Capture todo o tráfego:</p>
    <pre><code>sudo tcpdump -i eth0 -w captura-virus.pcap</code></pre>
    <p><em>(substitua <code>eth0</code> pelo nome da interface)</em></p>

  <h2>🔹 5. Analisar com Wireshark</h2>
    <ul>
        <li>Transfira <code>captura-virus.pcap</code> para seu PC seguro.</li>
        <li>Abra no Wireshark.</li>
        <li>Verifique:
            <ul>
                <li>Domínios acessados</li>
                <li>IPs suspeitos</li>
                <li>Tentativas de comando e controle (C2)</li>
            </ul>
        </li>
    </ul>

  <h2>🔹 Fluxo Resumido</h2>
    <ol>
        <li>Criar rede NAT isolada</li>
        <li>Conectar VM infectada</li>
        <li>Conectar VM Sniffer</li>
        <li>Capturar pacotes com tcpdump</li>
        <li>Analisar no Wireshark</li>
    </ol>

   <h2>🔒 Segurança Extra</h2>
    <ul>
        <li>Desative pastas compartilhadas no VirtualBox.</li>
        <li>Desative arrastar e soltar.</li>
        <li>Use snapshot antes do teste para restaurar rapidamente.</li>
        <li>Não mantenha a VM infectada ligada após o teste.</li>
    </ul>
