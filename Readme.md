LeiloesTDSat
Possuí o propósito de visualisar o histórico de vendas, dizendo quais produtos foram vendidos e quais ainda não foram além de dizer o valor dos produtos. Existem duas telas, a de cadastro e a de visualização, na tela de cadastro é possível ver duas caixas de texto que permitem a adição do nome e do preço de um produto que quando cadastrado automaticamente o status de "A Venda", também se encontra um botão que permite ir a tela de visualização que mostra uma tabela que mostra os produtos(id, nome, preço e status) e um botão com uma caixa de texto que permite com que seja alterado o status do produto, nesta tela também existe um botão que permite ir a tela de cadastro.
Este projeto é feito na linguagem java para se comunicar com um banco de dados sql(mysql).

https://melodic-profiterole-c4e923.netlify.app


<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>3d de Ar</title>
    <script type="module" src="https://cdn.jsdelivr.net/npm/@google/model-viewer@4.3.1/dist/model-viewer.js"></script>
    <style>
        body{
            margin:0;
            height:100dvh;
            overflow: hidden;
            background: #660138;
            color: #8856fd;
            font-family: sans-serif;
        }
        model-viewer
        {
            width: 100%;
            height: 100%;
        }
        button
        {
            background: #ad1b6c;
            color: #8856fd;
            border: 0;
            border-radius: 6px;
            padding: 8px 14px;
            font-size: 14px;
        }
        .botao-ar, .aviso-ar
        {
            position: absolute;
            bottom: 24px;
            left: 50%;
            transform: translateX(-50%);
        }
        .aviso-ar
        {
            width: 90%;
            text-align: center;
            font-size: 14px;
        }
        .status
        {
            position: absolute;
            inset: 0;
            display: grid;
            place-items: center;
            background: #660138;
            z-index: 1;
            pointer-events: none;
        }
        .status.hidden{
            display: none;
        }
        .status.error{
            color:#016644
        }
    </style>
</head>
<body>
    <model-viewer id="viewer"
        src="modelo.glb"
        alt="Aranha"
        ar
        ar-modes="webxr scene-viewer quick-look"
        ar-placement="floor"
        ar-scale="auto" 
        xr-environment
        camera-controls
        auto-rotate
        shadow-intensity="1"
        exposure="1"
        <button slot="ar-button" class="botao-ar">Ver em RA</button>>
    </model-viewer>
    <div id="aviso-ar" class="aviso-ar" hidden></div>
    <div id="status" class="status">Procurando modelo</div>
    <script>
    const viewer = document.getElementById('viewer')
    const status = document.getElementById('status')
    const avisoAR = document.getElementById('aviso-ar')
    function mostrarErro(mensagem){
        status.classList.remove('hidden')
        status.classList.add('error')
        status.textContent = mensagem;
    }
    function verificarDisponibilidadeAR(){
        if(viewer.canActivateAR){
            avisoAR.hidden=true
            return
        }
        const ehCelular = /Android|iPhone|iPad|iPod/i.test(navigator.userAgent)||(navigator.platform==='MacIntel' && navigator.maxTouchPoints>1)
        if(!window.isSecureContext && ehCelular){
            avisoAR.textContent = 'A Realidade Aumentada exige HTTPS. Abra este site por um endereço https://.'
        } else if(!ehCelular){
            avisoAR.textContent='Para ver em Realidade Aumentada abra este endereço no celular'
        } else{
            avisoAR.textContent = 'Este navegador não oferece Realidade Aumentada. Utilize Chrome(Android) ou Safari(iPhone)'
        }
        avisoAR.hidden=false
    }
    viewer.addEventListener('load', ()=>{
        status.classList.add('hidden')
        verificarDisponibilidadeAR()
    })
    viewer.addEventListener('error', (evento)=>{
        mostrarErro('Não foi possível carregar o modelo. Verifique se o arquivo modelo.glb está na mesma pasta do index.html')
        console.error('Erro ao carregar modelo', viewer.src, evento.detail)
    })
    viewer.addEventListener('ar-status', (evento)=>{
        if(evento.detail.status==='failed'){
            mostrarErro('Não foi possível iniciar a Realidade Aumentada neste dispositivo ou navegador')
        }
    })
    setTimeout(()=>{
        if(!customElements.get('model-viewer')){
            mostrarErro('Não foi possível carregar a biblioteca. Verifique a conexão com a internet.')
        }
    }, 10000)
</script>
</body>
</html>
