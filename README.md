# Teste WDIO Android

Projeto de automação de testes para aplicações Android utilizando **WebdriverIO** e **Appium**, seguindo o padrão **Page Object Model (POM)** para testes de interface com melhor organização e manutenibilidade do código.

## 📋 Sobre o Projeto

Este é um framework de testes automatizados para validar funcionalidades de uma aplicação Android nativa/híbrida. O projeto utiliza:

- **WebdriverIO**: Framework de automação web/mobile baseado em JavaScript
- **Appium**: Servidor de automação para aplicações mobile
- **Mocha**: Framework de testes
- **Page Object Model**: Padrão de design para estruturar testes

### Estrutura do Projeto

```
teste-wdio-android/
├── app/
│   └── wdio.apk              # Aplicação Android a testar
├── chromedriver-mobile/       # ChromeDriver para testes em navegador
├── test/
│   ├── pageobjects/           # Page Objects (estrutura dos elementos)
│   │   ├── drag.page.js
│   │   ├── forms.page.js
│   │   ├── login.page.js
│   │   └── swipe.page.js
│   └── specs/                 # Testes (casos de teste)
│       ├── drag.spec.js
│       ├── forms.spec.js
│       ├── login.spec.js
│       └── swipe.spec.js
├── wdio.conf.js               # Configuração principal do WebdriverIO
├── jsconfig.json              # Configuração JavaScript
└── package.json               # Dependências do projeto
```

## 🔧 Requisitos

Antes de começar, certifique-se de ter instalado:

- **Node.js** (v14 ou superior) - [Download](https://nodejs.org/)
- **npm** (incluído com Node.js)
- **Appium** (v2.x) - Será instalado como dependência
- **Java Development Kit (JDK)** - Para executar Appium/Android
- **Android SDK** - Para emulador ou dispositivo real
- **Android Studio** (recomendado) - Para configurar emulador

### Verificar Instalação

```bash
node --version
npm --version
```

## 📦 Instalação

### 1. Clonar o Repositório

```bash
git clone https://github.com/EBAC-QE/teste-wdio-android.git
cd teste-wdio-android
```

### 2. Instalar Dependências

```bash
npm install
```

Isto instalará:
- `@wdio/cli` - Interface de linha de comando
- `@wdio/local-runner` - Executor local
- `@wdio/mocha-framework` - Framework Mocha
- `@wdio/appium-service` - Serviço Appium
- `@wdio/spec-reporter` - Reporter de relatórios
- `appium-uiautomator2-driver` - Driver de automação Android

### 3. Preparar o Emulador/Dispositivo

**Usando Emulador Android:**

1. Abra Android Studio
2. Crie ou inicie um dispositivo virtual
3. Certifique-se de que o dispositivo está rodando

**Usando Dispositivo Real:**

1. Ative "Depuração USB" nas configurações do dispositivo
2. Conecte via cabo USB
3. Verifique a conexão:

```bash
adb devices
```

### 4. Instalar a Aplicação de Testes

A aplicação `wdio.apk` já está incluída na pasta `app/`. A configuração no `wdio.conf.js` referencia este arquivo.

## 🚀 Executar os Testes

### Executar Todos os Testes

```bash
npm test
```

Ou:

```bash
npx wdio run ./wdio.conf.js
```

### Executar um Teste Específico

```bash
npm test -- --spec ./test/specs/login.spec.js
```

Ou:

```bash
npx wdio run ./wdio.conf.js --spec ./test/specs/login.spec.js
```

### Exemplos de Testes Disponíveis

| Teste | Arquivo | Descrição |
|-------|---------|-----------|
| Login | `test/specs/login.spec.js` | Valida funcionalidades de login |
| Formulários | `test/specs/forms.spec.js` | Testa preenchimento de formulários |
| Drag & Drop | `test/specs/drag.spec.js` | Testa ações de arrastar e soltar |
| Swipe | `test/specs/swipe.spec.js` | Testa gestos de deslize |

## 🏗️ Padrão Page Object Model (POM)

Este projeto segue o padrão **Page Object Model**, que organiza os testes em duas camadas:

### Page Objects (`test/pageobjects/`)

Contêm:
- Localizadores (seletores) dos elementos da página
- Métodos que representam ações do usuário

**Exemplo - login.page.js:**

```javascript
class LoginPage {
    get emailInput() { return $('~email'); }
    get passwordInput() { return $('~password'); }
    get loginButton() { return $('~login'); }
    
    async login(email, password) {
        await this.emailInput.setValue(email);
        await this.passwordInput.setValue(password);
        await this.loginButton.click();
    }
}
```

### Specs/Testes (`test/specs/`)

Contêm:
- Casos de teste (describe/it)
- Assertions e validações
- Fluxos de usuário

**Exemplo - login.spec.js:**

```javascript
import LoginPage from '../pageobjects/login.page.js';

describe('Login Tests', () => {
    it('should login with valid credentials', async () => {
        await LoginPage.login('user@example.com', 'password123');
        expect(await LoginPage.successMessage).toBeDisplayed();
    });
});
```

### Benefícios do POM

✅ **Manutenibilidade**: Alterações de UI em um único lugar  
✅ **Reutilização**: Métodos compartilhados entre testes  
✅ **Legibilidade**: Testes mais claros e descritivos  
✅ **Escalabilidade**: Fácil adicionar novos testes  

## ⚙️ Configuração (wdio.conf.js)

O arquivo `wdio.conf.js` contém a configuração principal:

```javascript
// Capabilities - Configuração do dispositivo/emulador
capabilities: [{
    platformName: "Android",
    deviceName: "Medium Phone",           // Nome do emulador
    automationName: "uiautomator2",       // Driver de automação
    app: "app/wdio.apk",                  // Caminho do APK
    platformVersion: "9.0"                // Versão do Android
}]

// Timeout padrão para operações
waitforTimeout: 10000

// Service Appium
services: ['appium']

// Port do Appium
port: 4723
```

### Personalizações Comuns

**Mudar Dispositivo:**

Edite `capabilities.deviceName` para o seu emulador

**Aumentar Timeout:**

Modifique `waitforTimeout` (em ms)

**Usar Dispositivo Real:**

Certifique-se que o dispositivo está conectado e listado em `adb devices`

## 📊 Relatórios

Após executar os testes, um relatório será gerado. Os resultados podem ser visualizados no console.

## 🛠️ Troubleshooting

### Appium não inicia

```bash
# Inicie o Appium manualmente
npx appium
```

### Dispositivo não encontrado

```bash
# Verifique dispositivos conectados
adb devices

# Se não aparecer, reconecte o cabo ou reinicie o emulador
```

### Testes timeout

- Aumente `waitforTimeout` em `wdio.conf.js`
- Verifique se o emulador/dispositivo está responsivo
- Verifique se o APK está instalado corretamente

### Elementos não encontrados

- Verifique os seletores (accessibility IDs) nos page objects
- Use ferramentas como **Appium Inspector** para inspecionar elementos
- Certifique-se de que a aplicação está carregada

## 📚 Referências e Recursos

- [WebdriverIO Documentation](https://webdriver.io/)
- [Appium Documentation](https://appium.io/)
- [Mocha Framework](https://mochajs.org/)
- [Page Object Model Pattern](https://webdriver.io/docs/pageobjects/)
- [Android SDK Documentation](https://developer.android.com/docs)

## 👨‍💻 Autor

Desenvolvido ppor Fábio Araújo
Professor de Engenharia de Qualidade na EBAC
Linkedin: https://www.linkedin.com/in/fabio10/ 

## 📝 Licença

Este projeto é fornecido como está para fins educacionais.