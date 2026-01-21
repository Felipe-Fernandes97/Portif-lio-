# Documentação do Portfolio - Felipe Fernandes

## Resumo da Sessão

Esta documentação detalha todas as alterações e implementações realizadas no portfolio de Felipe Fernandes durante a sessão de desenvolvimento.

---

## 1. Objetivo Principal

Transformar a seção de projetos de um layout de grid estático para um **carousel dinâmico** que exibe projetos com imagens e vídeos, permitindo navegação fluida entre eles.

---

## 2. Estrutura Inicial

### Antes das Alterações
- **Layout**: Grid de cards fixos
- **Navegação**: Sem navegação entre projetos
- **Mídia**: Placeholders SVG para a maioria dos projetos

---

## 3. Implementação do Carousel

### 3.1 Estrutura HTML
Criamos uma nova seção de projetos com carousel:

```html
<section class="project-showcase fullpage-section" id="section-2" data-scroll-section>
    <h2>Projetos</h2>
    <div class="main-carousel-container">
        <div class="carousel-track" id="carousel-track"></div>
        <div class="global-controls" id="global-controls"></div>
    </div>
</section>
```

### 3.2 CSS Principal

#### Carousel Container
- **Arquivo**: `Porti/index.html` (linhas 355-362)
- **Características**:
  - `min-height: 600px`
  - `overflow: hidden` para ocultar slides fora da visualização
  - `padding-bottom: 100px` para espaço dos controles

#### Carousel Track
- **Arquivo**: `Porti/index.html` (linhas 364-367)
- **Características**:
  - `display: flex` para layout horizontal
  - `transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1)` para animação suave

#### Project Slide
- **Arquivo**: `Porti/index.html` (linhas 369-379)
- **Layout**: Grid com 2 colunas
  - Coluna 1 (1fr): Informações do projeto
  - Coluna 2 (1.5fr): Mídia (imagem/vídeo)

### 3.3 Controles de Navegação

#### Bolinhas de Navegação (Project Dots)
- **Localização**: `Porti/index.html` (linhas 506-544)
- **Características**:
  - Posicionamento absoluto no bottom center
  - Estado ativo com borda expandida (40px de largura)
  - Efeitos hover com scale e mudança de cor
  - Background semi-transparente com backdrop blur

#### Setas de Navegação de Mídia
- **Localização**: `Porti/index.html` (linhas 481-503)
- **Características**:
  - Aparecem apenas quando há múltiplas mídias por projeto
  - Posicionamento absoluto nos lados do container
  - Botões circulares com ícones FontAwesome

---

## 4. Sistema de Projetos

### 4.1 Estrutura de Dados

```javascript
const featuredProjects = [
    {
        title: "Nome do Projeto",
        category: "Categoria",
        desc: "Descrição detalhada",
        techs: ["Tech1", "Tech2", "Tech3"],
        links: {
            demo: "#",
            code: "#"
        },
        media: [
            { type: "video", src: "caminho/arquivo.mp4" },
            { type: "image", src: "caminho/imagem.png" }
        ]
    }
]
```

### 4.2 Projetos Configurados

| # | Projeto | Tipo | Arquivo | Status |
|---|---------|------|---------|--------|
| 1 | **Talents-MultiOne** | Imagem | `TalentsMulti/telaLoginMultiTalents.png` | ✅ |
| 2 | **DashBoard Empresarial** | Vídeo | `DashBoard/dashboard.mp4` | ✅ |
| 3 | **Saas MacDonald** | SVG Placeholder | - | ⚠️ Pendente |
| 4 | **Central Aviso** | Vídeo | `centralDeAviso/Gravando 2026-01-20 154709.mp4` | ✅ |
| 5 | **Software-House** | Vídeo | `soft/software-house.mp4` | ✅ |
| 6 | **Pacman Web** | Vídeo | `pacmen/pacmen.mp4` | ✅ |

---

## 5. Funcionalidades JavaScript

### 5.1 Renderização dos Slides

**Função**: `featuredProjects.forEach()` (linhas 930-981)

**Processo**:
1. Para cada projeto, cria HTML dinâmico
2. Renderiza mídia (imagem ou vídeo) com base no tipo
3. Adiciona tech pills (tecnologias usadas)
4. Configura visibilidade das setas de navegação
5. Insere o slide no carousel track

### 5.2 Navegação entre Projetos

**Função**: `goToProject(index)` (linhas 1008-1018)

```javascript
function goToProject(index) {
    currentProject = index;
    updateCarousel();
}
```

**Função**: `updateCarousel()` (linhas 1011-1018)

```javascript
function updateCarousel() {
    carouselTrack.style.transform = `translateX(-${currentProject * 100}%)`;

    const dots = globalControls.querySelectorAll('.project-dot');
    dots.forEach((dot, index) => {
        dot.classList.toggle('active', index === currentProject);
    });
}
```

### 5.3 Navegação entre Mídias

**Função**: `changeMedia(projectIndex, direction)` (linhas 1047-1061)

**Características**:
- Só funciona se houver mais de 1 mídia
- Usa aritmética modular para loop infinito
- Aplica transformação CSS para deslizar entre mídias
- Logging para debug

```javascript
function changeMedia(projectIndex, direction) {
    const totalMedia = featuredProjects[projectIndex].media.length;

    if (totalMedia <= 1) return;

    currentMediaIndexes[projectIndex] =
        (currentMediaIndexes[projectIndex] + direction + totalMedia) % totalMedia;

    const wrapper = document.getElementById(`media-wrapper-${projectIndex}`);
    if (wrapper) {
        wrapper.style.transform =
            `translateX(-${currentMediaIndexes[projectIndex] * 100}%)`;
    }
}
```

---

## 6. Estilos de Mídia

### 6.1 Container de Mídia

```css
.media-showcase {
    position: relative;
    width: 100%;
    height: 500px;
    animation: fadeInRight 0.6s ease-out;
    overflow: hidden;
}
```

### 6.2 Wrapper de Mídia

```css
.media-wrapper {
    display: flex;
    width: 100%;
    height: 100%;
    transition: transform 0.5s ease-in-out;
    border-radius: 20px;
    overflow: hidden;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.4);
}
```

### 6.3 Itens de Mídia

#### Imagens
```css
.media-item img {
    max-width: 100%;
    max-height: 100%;
    width: auto;
    height: auto;
    object-fit: contain;
    display: block;
}
```

#### Vídeos
```css
.media-item video {
    width: 100%;
    height: 100%;
    object-fit: contain;
}
```

**Nota**: Vídeos têm controles nativos do navegador (`controls` attribute).

---

## 7. Debugging e Logging

### 7.1 Console Logs Implementados

**Localização**: Linhas 984-998

**Informações Exibidas**:
- Total de projetos renderizados
- Largura total do carousel track
- Largura do container
- Para cada projeto:
  - Nome do projeto
  - Número de imagens encontradas
  - URL de cada imagem
  - Dimensões de cada imagem

### 7.2 Logging de Navegação

```javascript
console.log(`📸 Projeto ${projectIndex}: Navegando para imagem ${currentMediaIndexes[projectIndex]} de ${totalMedia}`);
```

---

## 8. Problemas Encontrados e Soluções

### 8.1 Imagens Não Apareciam

**Problema**: As imagens estavam carregando mas não eram visíveis.

**Causa**: CSS `object-fit: cover` estava cortando as imagens, e faltava `display: block`.

**Solução**:
```css
.media-item img {
    max-width: 100%;
    max-height: 100%;
    width: auto;
    height: auto;
    object-fit: contain;  /* Mudado de 'cover' para 'contain' */
    display: block;        /* Adicionado */
}
```

### 8.2 Navegação de Mídia Funcionando Incorretamente

**Problema**: O sistema estava reconhecendo o número correto de mídias, mas não estava exibindo.

**Diagnóstico**: Console logs mostraram que:
- Talents-MultiOne tinha 2 imagens (navegava 1 vez) ✅
- DashBoard tinha 3 imagens (navegava 2 vezes) ✅

**Solução**: Após investigação, decidimos usar vídeos ao invés de múltiplas imagens, simplificando o sistema para 1 mídia por projeto.

### 8.3 Caminhos de Arquivo Incorretos

**Problema**: Pasta "pacman" não existia, era "pacmen".

**Solução**:
```javascript
// Antes
{ type: "video", src: "pacman/pacman-web.mp4" }

// Depois
{ type: "video", src: "pacmen/pacmen.mp4" }
```

---

## 9. Estrutura de Pastas

```
Porti/
├── index.html
├── bg/
│   └── fundo.jpg
├── Faceimage/
│   └── euteste.jpg
├── iconSupa/
│   └── icons8-supabase-48.png
├── TalentsMulti/
│   ├── telaLoginMultiTalents.png
│   └── telaHomeMultiTalents.png
├── DashBoard/
│   └── dashboard.mp4
├── centralDeAviso/
│   └── Gravando 2026-01-20 154709.mp4
├── soft/
│   └── software-house.mp4
└── pacmen/
    └── pacmen.mp4
```

---

## 10. Tech Stack Marquee

### 10.1 Skills Configuradas

```javascript
const skills = [
    { name: "JavaScript", icon: "fa-brands fa-js" },
    { name: "TypeScript", icon: "fa-solid fa-code" },
    { name: "React", icon: "fa-brands fa-react" },
    { name: "Next.js", icon: "fa-solid fa-n" },
    { name: "Node.js", icon: "fa-brands fa-node-js" },
    { name: "Python", icon: "fa-brands fa-python" },
    { name: "Supabase", icon: "iconSupa/icons8-supabase-48.png", isImage: true },
    { name: "PostgreSQL", icon: "fa-solid fa-database" },
    { name: "HTML", icon: "fa-brands fa-html5" },
    { name: "CSS", icon: "fa-brands fa-css3-alt" },
    { name: "TailwindCSS", icon: "fa-solid fa-wind" },
    { name: "Git/GitHub", icon: "fa-brands fa-github" },
    { name: "Eng. Prompt", icon: "fa-solid fa-brain" }
];
```

### 10.2 Animação Marquee

**Características**:
- Duas linhas independentes
- Primeira linha: scroll da esquerda para direita
- Segunda linha: scroll da direita para esquerda
- Pausa ao hover
- Loop infinito com duplicação de conteúdo

```css
@keyframes scroll-left {
    0% { transform: translateX(0); }
    100% { transform: translateX(-50%); }
}

@keyframes scroll-right {
    0% { transform: translateX(-50%); }
    100% { transform: translateX(0); }
}
```

---

## 11. Integração Locomotive Scroll

### 11.1 Configuração

```javascript
window.addEventListener('DOMContentLoaded', () => {
    scroll = new LocomotiveScroll({
        el: document.querySelector('[data-scroll-container]'),
        // ... configurações
    });
});
```

### 11.2 Atributos Data

- `data-scroll`: Marca elementos para animação
- `data-scroll-section`: Define seções
- `data-scroll-speed`: Controla velocidade do parallax
- `data-scroll-delay`: Atraso na animação

---

## 12. Paleta de Cores

```css
:root {
    --bg-dark: #050508;
    --purple-primary: #9ca3af;
    --purple-glow: #e5e7eb;
    --text-main: #ffffff;
    --text-dim: #a0a0b8;
    --accent-lilac: #f3f4f6;
}
```

**Tema**: Tons de cinza com acentos sutis, mantendo profissionalismo.

---

## 13. Responsividade

### 13.1 Breakpoints Principais

- **Desktop**: Grid com 2 colunas (1fr + 1.5fr)
- **Padding**: 40px 80px nos slides
- **Gap**: 80px entre as colunas

**Nota**: Implementações mobile ainda não foram completamente otimizadas nesta sessão.

---

## 14. Experience Section

### 14.1 Layout

- Grid 2 colunas
- Cards com efeito "blob" animado
- Outline que muda de cor no hover
- Transform translateY(-10px) no hover

### 14.2 Blob Animation

```css
@keyframes blob-bounce {
    0% { transform: translate(-100%, -100%) translate3d(0, 0, 0); }
    /* ... mais keyframes */
}
```

---

## 15. Próximos Passos (Sugestões)

### 15.1 Pendências
- [ ] Adicionar mídia para **Saas MacDonald**
- [ ] Otimizar para dispositivos móveis
- [ ] Adicionar links reais para demos e código
- [ ] Implementar navegação por teclado (setas)

### 15.2 Melhorias Sugeridas
- [ ] Adicionar loading states para vídeos
- [ ] Implementar lazy loading para mídia
- [ ] Adicionar thumbnails para pré-visualização
- [ ] Criar sistema de filtros por tecnologia
- [ ] Adicionar animações de entrada/saída mais elaboradas

---

## 16. Comandos Úteis

### 16.1 Para Verificar Arquivos
```bash
ls -la "C:\Users\Multi360\Documents\0000000001\Porti\[PASTA]"
```

### 16.2 Para Encontrar Pastas
```bash
find "C:\Users\Multi360\Documents\0000000001\Porti" -type d -iname "*[BUSCA]*"
```

---

## 17. Contatos e Links

- **Título**: Felipe Fernandes | Full Stack & AI Engineer
- **GitHub**: (Links a serem adicionados)
- **Demo Links**: (Links a serem adicionados)

---

## 18. Licença e Créditos

### Bibliotecas Utilizadas
- **Locomotive Scroll** v4.1.4
- **Font Awesome** v6.4.2

### Ícones
- Supabase icon por Icons8

---

## Changelog

### [2026-01-20]
- ✅ Implementado carousel de projetos
- ✅ Adicionados 4 vídeos de projetos
- ✅ Configurado sistema de navegação
- ✅ Corrigidos problemas de exibição de mídia
- ✅ Adicionado debugging console
- ✅ Atualizado Central Aviso com vídeo
- ✅ Documentação completa criada

---

**Fim da Documentação**

*Última atualização: 20 de Janeiro de 2026*
