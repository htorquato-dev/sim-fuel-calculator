# ACC Fuel Calculator

Calculadora web para estimar o combustível necessário para completar uma corrida no **Assetto Corsa Competizione**. Informe a duração da prova, o tempo médio por volta e o consumo de combustível; o resultado é arredondado para o próximo litro inteiro.

## Recursos

- Cálculo do combustível com base na duração da corrida e no consumo por volta.
- Opções para adicionar uma volta de apresentação e uma volta extra.
- Validação dos valores informados e indicação de erros.
- Interface responsiva em português.

## Tecnologias

- Vue 3
- Vite 6
- JavaScript e CSS

## Requisitos

- Node.js 18 ou superior
- npm

## Instalação e uso

Clone o repositório e instale as dependências:

```bash
git clone https://github.com/htorquato-dev/sim-fuel-calculator.git
cd sim-fuel-calculator
npm install
```

Inicie o servidor de desenvolvimento:

```bash
npm run dev
```

O Vite exibirá no terminal o endereço local para abrir no navegador.

## Outros comandos

```bash
npm run build    # Gera a versão de produção em dist/
npm run preview  # Abre localmente a versão de produção
```

## Como o cálculo funciona

O tempo da corrida e o tempo por volta são convertidos para segundos. A quantidade base é calculada pela proporção entre esses tempos e o consumo informado por volta. Cada opção de volta adicional soma mais uma volta de consumo. Ao final, o total é arredondado para cima ao litro inteiro seguinte.

## Estrutura do projeto

```text
.
├── public/          # Arquivos estáticos
├── src/
│   ├── assets/      # Recursos usados pela aplicação
│   ├── App.vue      # Interface e cálculo
│   ├── main.js      # Inicialização do Vue
│   └── styles.css   # Estilos
├── index.html
├── package.json
└── vite.config.js
```

## Licença

Nenhuma licença foi especificada neste repositório.
