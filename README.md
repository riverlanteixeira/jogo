# 🎮 Stranger Things AR - Caça aos Objetos

Um jogo de **Realidade Aumentada (RA)** imersivo baseado na série Stranger Things, onde você deve encontrar e coletar objetos icônicos da série espalhados pelo mundo real usando seu celular.

![Stranger Things Logo](assets/img/stranger-things-logo.png)

## 🌟 Sobre o Jogo

Entre no **Mundo Invertido** e ajude os personagens de Hawkins a recuperar objetos perdidos! Este jogo utiliza tecnologia de realidade aumentada para navegadores móveis, permitindo que você veja e interaja com objetos 3D do universo Stranger Things no seu ambiente real.

### 🎯 Objetivo
Encontre e colete **7 objetos icônicos** da série:
- 📻 **Walkie-Talkie** - Para comunicação com o grupo
- 🚲 **Bicicleta do Will** - O meio de transporte favorito dos garotos
- 🧭 **Bússola** - Para navegar pelo Mundo Invertido
- ⚾ **Taco de Baseball** - Arma de defesa contra criaturas
- 🪓 **Machado** - Ferramenta de sobrevivência
- 👹 **Demogorgon** - A criatura do Mundo Invertido
- 👦 **Will (Mundo Invertido)** - Encontre Will perdido na dimensão alternativa

## 🚀 Tecnologias Utilizadas

### Realidade Aumentada
- **WebXR Device API** - API moderna para experiências de RA na web
- **A-Frame 1.5.0** - Framework para criar experiências de realidade virtual e aumentada
- **Three.js** - Biblioteca 3D para renderização de modelos e cenas
- **Hit-Test API** - Para detecção de superfícies no mundo real

### Recursos Avançados
- **Geolocalização GPS** - Sistema de proximidade baseado em localização real
- **Modelos 3D otimizados** - Formato GLB para carregamento rápido
- **Interface responsiva** - Design adaptado para dispositivos móveis
- **Sistema de notificações** - Feedback visual e sonoro para o jogador
- **Carregamento offline** - Todos os assets são pré-carregados para jogo offline

## 📱 Compatibilidade

### Navegadores Suportados
- **Android**: Google Chrome 79+ (recomendado)
- **iOS**: Safari 13+ (com suporte limitado via AR Quick Look)

### Requisitos do Dispositivo
- Smartphone com sensor de movimento (giroscópio/acelerômetro)
- Câmera traseira funcional
- Conexão com internet para carregar assets
- GPS ativado para funcionalidades de localização

## 🎮 Como Jogar

### 1. Inicialização
1. Abra o jogo no navegador do seu celular
2. Aguarde o carregamento dos modelos 3D
3. Clique em **"Entrar no Mundo Invertido"**
4. Permita acesso à câmera e localização quando solicitado

### 2. Gameplay
1. **Receba a ligação inicial do Dustin** - Atenda para receber as primeiras instruções
2. **Explore o ambiente** - Mova seu celular para procurar objetos
3. **Use o medidor de distância** - Indica quando você está próximo de um item
4. **Toque para coletar** - Clique nos objetos quando estiverem visíveis
5. **Receba novas instruções** - Dustin liga após cada item coletado com novas orientações
6. **Acompanhe o progresso** - Use a mochila (🎒) para ver itens coletados

### 3. Interface do Jogo
- **Mochila** (canto superior direito) - Visualize progresso e itens coletados
- **Medidor de distância** (parte inferior) - Mostra proximidade dos objetos
- **Notificações toast** - Feedback sobre ações e descobertas
- **Retícula vermelha** - Indica onde objetos podem ser posicionados

## 🛠️ Estrutura do Projeto

```
stranger-things-ar/
├── index.html              # Arquivo principal do jogo
├── ok.html                 # Versão alternativa/teste
├── assets/
│   ├── audio/
│   │   ├── dustin-missao-1-completa.mp3    # Áudio inicial (Walkie-Talkie)
│   │   ├── dustin-missao-2-completa.mp3    # Áudio após Bicicleta
│   │   ├── dustin-missao-3-completa.mp3    # Áudio após Bússola
│   │   ├── dustin-missao-4-completa.mp3    # Áudio após Taco de Baseball
│   │   ├── dustin-missao-5-completa.mp3    # Áudio após Machado
│   │   ├── dustin-missao-6-completa.mp3    # Áudio após Demogorgon
│   │   └── dustin-missao-7-completa.mp3    # Áudio após Will
│   ├── img/
│   │   ├── dustin.png             # Avatar do Dustin
│   │   └── stranger-things-logo.png
│   └── models/                    # Modelos 3D em formato GLB
│       ├── baseball_bat.glb
│       ├── bicicleta-will.glb
│       ├── compass.glb
│       ├── demogorgon.glb
│       ├── machado.glb
│       ├── walkie_talkie.glb
│       └── will_byers_mundo_invertido.glb
└── README.md
```

## 🔧 Funcionalidades Técnicas

### Sistema de Hit-Test
O jogo utiliza a **WebXR Hit-Test API** para detectar superfícies reais onde os objetos podem ser posicionados:

```javascript
// Detecção de superfícies horizontais
xrSession.requestHitTestSource({ space: referenceSpace })
  .then((source) => {
    hitTestSource = source;
  });
```

### Sistema de Carregamento Real
Todos os assets são carregados antes do jogo iniciar, garantindo experiência offline:

```javascript
function startRealLoading() {
  const modelAssets = ['bike-model', 'compass-model', ...];
  const audioAssets = Object.values(dustinAudios);
  
  totalAssets = modelAssets.length + audioAssets.length;
  
  // Carregar modelos 3D
  modelAssets.forEach(assetId => {
    document.getElementById(assetId).addEventListener('loaded', updateProgress);
  });
  
  // Carregar áudios
  Object.keys(dustinAudios).forEach(key => {
    const audio = new Audio(dustinAudios[key]);
    audio.addEventListener('canplaythrough', updateProgress);
    preloadedAudios[key] = audio;
  });
}
```

### Sistema de Ligações Progressivas
Cada objeto coletado dispara uma nova ligação do Dustin com instruções específicas:

```javascript
const dustinAudios = {
  'walkie-talkie': 'assets/audio/dustin-missao-1-completa.mp3',
  'bike': 'assets/audio/dustin-missao-2-completa.mp3',
  'compass': 'assets/audio/dustin-missao-3-completa.mp3',
  // ... demais objetos
};

function collectItem(id) {
  if (dustinAudios[id]) {
    setTimeout(() => showDustinCall(id), 3000);
  }
}
```

### Geolocalização Inteligente
Sistema de proximidade que calcula distância entre jogador e objetos virtuais:
- Objetos aparecem quando você está dentro do raio de detecção
- Medidor de distância fornece feedback visual
- GPS usado para posicionamento preciso dos itens

### Componentes A-Frame Customizados
```javascript
AFRAME.registerComponent('collectible-object', {
  // Lógica para objetos colecionáveis
  // Detecção de cliques e interações
  // Animações e feedback visual
});
```

## 🎨 Design e UX

### Tema Visual
- **Paleta de cores**: Vermelho Stranger Things (#e72626), preto e branco
- **Tipografia**: Helvetica Neue para legibilidade em dispositivos móveis
- **Animações**: Efeitos de pulsação, ondas e transições suaves
- **Ícones**: Emojis temáticos para identificação rápida dos itens

### Experiência do Usuário
- **Carregamento real** com progresso detalhado de todos os assets
- **Jogo offline** após carregamento inicial completo
- **Feedback háptico** através de animações e sons
- **Interface minimalista** para não interferir na experiência AR
- **Sistema de notificações** não-intrusivo

## 🚀 Como Executar

### Desenvolvimento Local
1. Clone ou baixe o projeto
2. Use um servidor HTTP local (não funciona com file://)
3. Recomendado: `python -m http.server 8000` ou similar
4. Acesse via HTTPS (necessário para WebXR)

### Servidor Web
1. Faça upload dos arquivos para seu servidor
2. Certifique-se de que o servidor suporta HTTPS
3. Configure headers CORS se necessário
4. Teste em dispositivos móveis reais

## 🔍 Solução de Problemas

### Problemas Comuns

**Câmera não funciona:**
- Verifique permissões do navegador
- Certifique-se de estar usando HTTPS
- Teste em modo privado/incógnito

**Objetos não aparecem:**
- Verifique se o GPS está ativado
- Mova-se pelo ambiente físico
- Aguarde o carregamento completo dos modelos

**Performance baixa:**
- Feche outras abas do navegador
- Reinicie o aplicativo
- Verifique conexão com internet

### Logs de Debug
O jogo inclui logs detalhados no console do navegador para diagnóstico de problemas.

## 🎯 Próximas Funcionalidades

- [ ] **Modo Multiplayer** - Jogue com amigos
- [ ] **Mais objetos** - Expansão com itens da 4ª temporada
- [ ] **Sistema de conquistas** - Badges e recompensas
- [ ] **Modo história** - Missões narrativas sequenciais
- [ ] **Realidade aumentada social** - Compartilhamento de descobertas

## 📄 Licença

Este projeto é um fan-game educacional baseado na série Stranger Things. Todos os direitos da propriedade intelectual pertencem à Netflix e aos criadores da série.

## 🤝 Contribuições

Contribuições são bem-vindas! Sinta-se à vontade para:
- Reportar bugs
- Sugerir melhorias
- Adicionar novos recursos
- Otimizar performance

---

**Desenvolvido com ❤️ para fãs de Stranger Things e entusiastas de Realidade Aumentada**

*"Friends don't lie... and neither does AR!"* 🎮👾