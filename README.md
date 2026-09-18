[script.js](https://github.com/user-attachments/files/32393797/script.js)
/**
 * Portfolio Oficial - Kauã Lopes Rodrigues
 * Script Interativo: Terminal CLI, Simulador de Orçamento, Filtros e Tema
 */

document.addEventListener('DOMContentLoaded', () => {
  initThemeToggle();
  initMobileMenu();
  initInteractiveTerminal();
  initProjectFilters();
  initSimulator();
  initContactForm();
  initSmoothScroll();
});

/* ==========================================================================
   1. TEMA DARK / LIGHT (Persistência em localStorage)
   ========================================================================== */
function initThemeToggle() {
  const themeToggle = document.getElementById('themeToggle');
  const themeIcon = document.getElementById('themeIcon');
  const htmlRoot = document.documentElement;

  // Carregar tema salvo ou preferência do sistema
  const savedTheme = localStorage.getItem('kaua_portfolio_theme');
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  const currentTheme = savedTheme || (prefersDark ? 'dark' : 'light');

  applyTheme(currentTheme);

  if (themeToggle) {
    themeToggle.addEventListener('click', () => {
      const activeTheme = htmlRoot.getAttribute('data-theme') || 'dark';
      const newTheme = activeTheme === 'dark' ? 'light' : 'dark';
      applyTheme(newTheme);
    });
  }

  function applyTheme(theme) {
    htmlRoot.setAttribute('data-theme', theme);
    localStorage.setItem('kaua_portfolio_theme', theme);
    if (themeIcon) {
      themeIcon.textContent = theme === 'dark' ? '☀️' : '🌙';
      themeToggle.setAttribute('title', theme === 'dark' ? 'Mudar para modo claro' : 'Mudar para modo escuro');
    }
  }
}

/* ==========================================================================
   2. MENU MOBILE
   ========================================================================== */
function initMobileMenu() {
  const mobileToggle = document.getElementById('mobileToggle');
  const navLinks = document.getElementById('navLinks');

  if (!mobileToggle || !navLinks) return;

  mobileToggle.addEventListener('click', () => {
    mobileToggle.classList.toggle('active');
    navLinks.classList.toggle('active');
  });

  // Fechar menu ao clicar em qualquer item
  navLinks.querySelectorAll('a').forEach(link => {
    link.addEventListener('click', () => {
      mobileToggle.classList.remove('active');
      navLinks.classList.remove('active');
    });
  });
}

/* ==========================================================================
   3. TERMINAL INTERATIVO (CLI kaua-terminal)
   ========================================================================== */
function initInteractiveTerminal() {
  const terminalInput = document.getElementById('terminalInput');
  const terminalOutput = document.getElementById('terminalOutput');

  if (!terminalInput || !terminalOutput) return;

  // Foco ao clicar em qualquer lugar do terminal
  const terminalCard = document.querySelector('.terminal-card');
  if (terminalCard) {
    terminalCard.addEventListener('click', () => {
      terminalInput.focus();
    });
  }

  const commands = {
    help: () => 
<div class="term-response">
  <span class="term-highlight">COMANDOS DISPONÍVEIS:</span><br>
  • <span class="term-key">skills</span>     : Tecnologias e ferramentas que domino<br>
  • <span class="term-key">projetos</span>   : Lista e detalhes dos projetos desenvolvidos<br>
  • <span class="term-key">sobre</span>      : Perfil profissional e trajetória<br>
  • <span class="term-key">contato</span>    : Informações diretas de e-mail e WhatsApp<br>
  • <span class="term-key">whatsapp</span>   : Inicia conversa imediata no WhatsApp<br>
  • <span class="term-key">orcamento</span>  : Rola até o simulador de investimento<br>
  • <span class="term-key">clear</span>      : Limpa a tela do terminal
</div>,

    ajuda: () => commands.help(),

    skills: () => 
<div class="term-response">
  <span class="term-highlight">STACK TÉCNICA PRINCIPAL:</span><br>
  • <span class="term-key">Linguagens:</span> Java (SE 8 ao 24), JavaScript (ES6+), SQL, HTML5, CSS3<br>
  • <span class="term-key">Desktop UI:</span> Java Swing, FlatLaf Modern Look&Feel, Padrão MVC<br>
  • <span class="term-key">Banco de Dados:</span> MySQL, PostgreSQL, Modelagem Relacional, Migrações ACID<br>
  • <span class="term-key">Web:</span> Landing pages responsivas, Glassmorphism, APIs REST, Vanilla JS<br>
  • <span class="term-key">Engenharia:</span> Inno Setup (Instaladores executáveis autônomos com MySQL embutido), Git
</div>,

    habilidades: () => commands.skills(),
    tech: () => commands.skills(),

    projetos: () => 
<div class="term-response">
  <span class="term-highlight">DESTAQUES DO PORTFÓLIO:</span><br>
  1. <span class="term-key">AgroLíder (Rancho da Figueira v3.7):</span> Gestão agropecuária, PDV misto, vacinação, livro caixa DRE e instalador silencioso.<br>
  2. <span class="term-key">Nexus PDV Comercial:</span> Frente de caixa para lojas e varejo em geral.<br>
  3. <span class="term-key">FluxoMaster:</span> DRE e livro caixa financeiro corporativo com balancetes.<br>
  <em>💡 Role a página para ver os cards interativos!</em>
</div>,

    portfolio: () => commands.projetos(),

    sobre: () => 
<div class="term-response">
  <span class="term-highlight">KAUÃ LOPES RODRIGUES</span><br>
  Desenvolvedor de Software focado em soluções comerciais, corporativas e digitais.<br>
  Atuação em <strong>qualquer segmento empresarial</strong>: desde o pequeno comércio de bairro até grandes controles operacionais e aplicações web modernas.<br>
  Compromisso com estabilidade, código limpo e sistemas fáceis de operar.
</div>,

    bio: () => commands.sobre(),

    contato: () => 
<div class="term-response">
  <span class="term-highlight">CANAIS DE ATENDIMENTO DIRETO:</span><br>
  • <span class="term-key">WhatsApp:</span> (53) 99146-7168<br>
  • <span class="term-key">E-mail:</span> kaua.lopes0307@gmail.com<br>
  • <span class="term-key">Cidade:</span> São Lourenço do Sul - RS (Atendimento presencial & remoto)<br>
  • <span class="term-cmd"><a href="https://wa.me/5553991467168?text=Olá%20Kauã,%20vi%20seu%20terminal!" target="_blank" style="color:var(--primary); text-decoration:underline;">Clique aqui para abrir o WhatsApp</a></span>
</div>,

    contact: () => commands.contato(),

    whatsapp: () => {
      window.open('https://wa.me/5553991467168?text=Olá%20Kauã,%20estava%20testando%20seu%20terminal%20e%20gostaria%20de%20conversar!', '_blank');
      return <div class="term-response"><span class="term-highlight">✓ Redirecionando para o WhatsApp...</span></div>;
    },

    whats: () => commands.whatsapp(),

    orcamento: () => {
      const simSection = document.getElementById('simulador');
      if (simSection) {
        simSection.scrollIntoView({ behavior: 'smooth' });
      }
      return <div class="term-response"><span class="term-highlight">✓ Rolando até o simulador de orçamento...</span></div>;
    },

    simulador: () => commands.orcamento(),

    sudo: () => 
<div class="term-response">
  <span class="term-highlight">kaua@dev:</span> Acesso concedido! Você tem privilégios totais para solicitar o melhor software para o seu negócio. 😉
</div>,

    clear: () => {
      terminalOutput.innerHTML = 
        <div class="term-line"><span class="term-prompt">kaua@dev:~$</span> <span class="term-cmd">help</span></div>
        <div class="term-comment">// Tela limpa. Digite comandos como: 'projetos', 'contato', 'skills', 'orcamento'</div>
      ;
      return null;
    },

    cls: () => commands.clear()
  };

  terminalInput.addEventListener('keydown', (e) => {
    if (e.key === 'Enter') {
      const rawInput = terminalInput.value.trim();
      if (!rawInput) return;

      const command = rawInput.toLowerCase();
      terminalInput.value = '';

      // Adicionar linha do comando digitado
      const cmdRow = document.createElement('div');
      cmdRow.className = 'term-line';
      cmdRow.innerHTML = <span class="term-prompt">kaua@dev:~$</span> <span class="term-cmd"></span>;
      terminalOutput.appendChild(cmdRow);

      // Processar comando
      if (commands[command]) {
        const output = commands[command]();
        if (output) {
          const respDiv = document.createElement('div');
          respDiv.innerHTML = output;
          terminalOutput.appendChild(respDiv);
        }
      } else {
        const errorDiv = document.createElement('div');
        errorDiv.className = 'term-response';
        errorDiv.innerHTML = <span style="color:#ef4444;">Comando não reconhecido: ''.</span> Digite <span class="term-key">help</span> para ver os comandos válidos.;
        terminalOutput.appendChild(errorDiv);
      }

      // Auto-scroll para a última linha
      terminalOutput.scrollTop = terminalOutput.scrollHeight;
    }
  });

  function escapeHtml(str) {
    return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
  }
}

/* ==========================================================================
   4. FILTRO DE PROJETOS
   ========================================================================== */
function initProjectFilters() {
  const filterBtns = document.querySelectorAll('.filter-btn');
  const projectCards = document.querySelectorAll('.project-card');

  if (!filterBtns.length || !projectCards.length) return;

  filterBtns.forEach(btn => {
    btn.addEventListener('click', () => {
      // Atualizar classe ativa no botão
      filterBtns.forEach(b => b.classList.remove('active'));
      btn.classList.add('active');

      const filterValue = btn.getAttribute('data-filter');

      projectCards.forEach(card => {
        const categories = card.getAttribute('data-category') || '';
        
        if (filterValue === 'all' || categories.split(' ').includes(filterValue)) {
          card.style.display = 'flex';
          card.style.opacity = '0';
          card.style.transform = 'translateY(15px)';
          setTimeout(() => {
            card.style.transition = 'all 0.35s ease';
            card.style.opacity = '1';
            card.style.transform = 'translateY(0)';
          }, 30);
        } else {
          card.style.display = 'none';
        }
      });
    });
  });
}

/* ==========================================================================
   5. SIMULADOR INTERATIVO DE PROJETOS E ORÇAMENTO
   ========================================================================== */
function initSimulator() {
  const projTypeGroup = document.getElementById('projTypeGroup');
  const pricingModelGroup = document.getElementById('pricingModelGroup');
  const extraCheckboxes = document.querySelectorAll('.sim-extra');

  // Elementos do Resumo
  const simModelBadge = document.getElementById('simModelBadge');
  const simSelectedType = document.getElementById('simSelectedType');
  const simEstimatedTime = document.getElementById('simEstimatedTime');
  const simExtrasCount = document.getElementById('simExtrasCount');
  const simTotalValue = document.getElementById('simTotalValue');
  const simFootnote = document.getElementById('simFootnote');
  const btnEnviarWhatsAppSim = document.getElementById('btnEnviarWhatsAppSim');

  if (!projTypeGroup || !pricingModelGroup) return;

  let currentBaseCost = 2800;
  let currentEstimatedTime = '3 semanas';
  let currentTypeDesc = 'Sistema Desktop Completo';
  let currentMultiplier = 1.0;
  let isSubscription = false;

  // Clique em Tipo de Projeto
  projTypeGroup.querySelectorAll('.sim-pill').forEach(pill => {
    pill.addEventListener('click', () => {
      projTypeGroup.querySelectorAll('.sim-pill').forEach(p => p.classList.remove('active'));
      pill.classList.add('active');

      currentBaseCost = parseFloat(pill.getAttribute('data-cost')) || 2800;
      currentEstimatedTime = pill.getAttribute('data-time') || '2 a 3 semanas';
      currentTypeDesc = pill.textContent.trim();

      updateSimulatorCalculation();
    });
  });

  // Clique em Modelo Comercial
  pricingModelGroup.querySelectorAll('.sim-pill').forEach(pill => {
    pill.addEventListener('click', () => {
      pricingModelGroup.querySelectorAll('.sim-pill').forEach(p => p.classList.remove('active'));
      pill.classList.add('active');

      currentMultiplier = parseFloat(pill.getAttribute('data-multiplier')) || 1.0;
      isSubscription = currentMultiplier < 1.0;

      updateSimulatorCalculation();
    });
  });

  // Mudança em Checkboxes de Extras
  extraCheckboxes.forEach(chk => {
    chk.addEventListener('change', () => {
      updateSimulatorCalculation();
    });
  });

  function updateSimulatorCalculation() {
    let extrasSum = 0;
    let selectedExtrasList = [];

    extraCheckboxes.forEach(chk => {
      if (chk.checked) {
        const val = parseFloat(chk.value) || 0;
        extrasSum += val;
        selectedExtrasList.push(chk.getAttribute('data-name') || 'Item Extra');
      }
    });

    let totalCalc = 0;
    if (isSubscription) {
      // Modelo SaaS mensal
      totalCalc = (currentBaseCost * currentMultiplier) + (extrasSum * 0.25);
    } else {
      // Compra única vitalícia
      totalCalc = currentBaseCost + extrasSum;
    }

    // Atualizar UI
    if (simModelBadge) {
      simModelBadge.textContent = isSubscription ? 'Assinatura Mensal' : 'Compra Única';
      simModelBadge.style.background = isSubscription ? 'rgba(59, 130, 246, 0.15)' : 'rgba(16, 185, 129, 0.15)';
      simModelBadge.style.color = isSubscription ? 'var(--secondary)' : 'var(--success)';
    }

    if (simSelectedType) {
      simSelectedType.textContent = currentTypeDesc;
    }

    if (simEstimatedTime) {
      simEstimatedTime.textContent = currentEstimatedTime;
    }

    if (simExtrasCount) {
      simExtrasCount.textContent = selectedExtrasList.length === 0 
        ? 'Nenhum adicional' 
        : ${selectedExtrasList.length} selecionado(s);
    }

    if (simTotalValue) {
      const formattedTotal = totalCalc.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
      simTotalValue.textContent = isSubscription ? ${formattedTotal} /mês : formattedTotal;
    }

    if (simFootnote) {
      simFootnote.textContent = isSubscription 
        ? 'Inclui suporte técnico contínuo, atualizações de versão e backup monitorado.' 
        : 'Licença perpétua sem mensalidades. Inclui instalação e 90 dias de suporte e garantia.';
    }

    // Configurar botão de envio para WhatsApp com a mensagem montada
    if (btnEnviarWhatsAppSim) {
      btnEnviarWhatsAppSim.onclick = () => {
        enviarSimulacaoWhatsApp(totalCalc, selectedExtrasList);
      };
    }
  }

  function enviarSimulacaoWhatsApp(total, extras) {
    const valorFormatado = total.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
    const modoTexto = isSubscription ? ${valorFormatado} /mês (Assinatura SaaS) : ${valorFormatado} (Pagamento Único Vitalício);
    const extrasTexto = extras.length > 0 ? extras.join(', ') : 'Nenhum recurso adicional';

    const textoMensagem = 
Olá Kauã! Estive no seu site e montei uma simulação de projeto:

*Tipo de Solução:* 
*Modelo Desejado:* 
*Prazo Estimado:* 
*Recursos Extras:* 
*Investimento Estimado:* 

Gostaria de conversar sobre como podemos desenvolver essa solução para o meu negócio!;

    const zapUrl = https://wa.me/5553991467168?text=;
    window.open(zapUrl, '_blank');
  }

  // Executar cálculo inicial
  updateSimulatorCalculation();
}

/* ==========================================================================
   6. FORMULÁRIO DE CONTATO (Envio direto para WhatsApp)
   ========================================================================== */
function initContactForm() {
  const contactForm = document.getElementById('contactForm');
  if (!contactForm) return;

  contactForm.addEventListener('submit', (e) => {
    e.preventDefault();

    const nome = document.getElementById('formNome')?.value.trim() || '';
    const telefone = document.getElementById('formTelefone')?.value.trim() || '';
    const assunto = document.getElementById('formAssunto')?.value || 'Contato Profissional';
    const mensagem = document.getElementById('formMensagem')?.value.trim() || '';

    if (!nome || !telefone || !mensagem) {
      alert('Por favor, preencha todos os campos obrigatórios.');
      return;
    }

    const textoWhatsApp = 
Olá Kauã, meu nome é **!
*Telefone/Whats:* 
*Assunto:* 

*Mensagem:*


(Enviado diretamente através do formulário do seu site);

    const zapUrl = https://wa.me/5553991467168?text=;

    // Feedback visual no botão
    const submitBtn = contactForm.querySelector('button[type="submit"]');
    const originalContent = submitBtn.innerHTML;
    submitBtn.innerHTML = <span>Redirecionando para o WhatsApp...</span>;
    submitBtn.style.background = 'var(--success)';

    setTimeout(() => {
      window.open(zapUrl, '_blank');
      submitBtn.innerHTML = originalContent;
      submitBtn.style.background = '';
      contactForm.reset();
    }, 600);
  });
}

/* ==========================================================================
   7. SCROLL SUAVE & DESTAQUE DE NAVEGAÇÃO
   ========================================================================== */
function initSmoothScroll() {
  document.querySelectorAll('a[href^="#"]').forEach(anchor => {
    anchor.addEventListener('click', function(e) {
      const targetId = this.getAttribute('href');
      if (targetId === '#') return;

      const targetElement = document.querySelector(targetId);
      if (targetElement) {
        e.preventDefault();
        const headerOffset = 80;
        const elementPosition = targetElement.getBoundingClientRect().top;
        const offsetPosition = elementPosition + window.pageYOffset - headerOffset;

        window.scrollTo({
          top: offsetPosition,
          behavior: 'smooth'
        });
      }
    });
  });

  // Efeito de sombra na barra de navegação ao rolar
  const navbar = document.getElementById('navbar');
  window.addEventListener('scroll', () => {
    if (!navbar) return;
    if (window.scrollY > 40) {
      navbar.style.boxShadow = '0 10px 30px rgba(0, 0, 0, 0.3)';
    } else {
      navbar.style.boxShadow = 'none';
    }
  });
}
