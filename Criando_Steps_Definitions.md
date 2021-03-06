# Criando seu primeiro arquivo de Steps.

Agora que criamos nosso arquivo .feature com Gherkin, vamos pegar o resultado obtido para criar nosso arquivo de Steps e vamos dar o nome de register_steps.rb na pasta step_definitions:

```ruby
workspace\capybaraforall\cucumber\features\step_definitions\register_steps.rb
```
E aquele resultado vamos copiar e colar nesse arquivo e vai ficar assim:

```ruby
Given(/^I access the Walmart site$/) do
  pending # Write code here that turns the phrase above into concrete actions
end

When(/^I click on Register$/) do
  pending # Write code here that turns the phrase above into concrete actions
end

And(/^fill in the fields$/) do
  pending # Write code here that turns the phrase above into concrete actions
end

And(/^click Cadastar$/) do
  pending # Write code here that turns the phrase above into concrete actions
end

Then(/^my registration will be successfully made$/) do
  pending # Write code here that turns the phrase above into concrete actions
end
```

Se você reparou, cada Given/When/And/Then + RegEx tem um "do" e um "end" ou seja, Given/When/And/Then da início ao método que vamos desenvolver, "do" te da a prerrogativa (essa eu gastei o português) para início do desenvolvimento (ou colocar os comandos para serem executados) e o "end" finaliza o método. Então podemos concluir que dentro de um arquivo de steps eu tenho um método para cada passo do meu teste. E indo além, quando eu, em qualquer outro arquivo.feature escrever uma mesma frase: "Given I access the Walmart site" eu não vou me preocupar em ter que desenvolver esse step de novo, porque ele já foi criado em algum lugar dentro de um método.

Bem .. arquivo criado, vamos ao que interessa, que é justamente dar vida ao nosso teste.

No lugar do "pending" vamos colocar os comandos necessários para execução dos passos do nosso teste. Vou listar aqui os comandos básicos para que possamos trabalhar dentro desse contexto.

Em outros artigos vou falar de técnicas mais específicas e com outros comandos, mas por hora pense que você está aprendendo e que precisa começar do zero, e se eu te passar um monte de comando, você vai se confundir e não vai entender, e fatalmente vai perder o interesse.

```ruby
visit "url do site" - Bom , não precisa falar muito dessa neh =)

fill_in ‘ID do elemento para inserir valor’, :with “Aprendendo Capybara” – Irá inserir no elemento a string Aprendendo Capybara.

find(:id, “id do elemento”).click – Clica em um elemento definido por ID.
find(:css, “css do elemento”).click – Clica em um elemento definido por CSS.
find(:xpath, “xpath do elemento”).click – Clica em um elemento definido por XPATH.

all(:id, “id do elemento”)[0].click – Clica no primeiro elemento dentro de uma lista definido por ID.
all(:css, “css do elemento”)[0].click – Clica no primeiro elemento dentro de uma lista definido por CSS.
all(:xpath, “xpath do elemento”)[0].click – Clica no primeiro elemento dentro de uma lista definido por XPATH.

PS: Quando nos depararmos com um checkbox, radiobutton, utilizar da seguinte forma:

find(:radio_button, ‘nome do radiobutton’).set(true) – Nesse caso, ele vai selecionar aquele radiobutton.
find(:checkbox, ‘nome do checkbox’).set(true) – Nesse caso, ele vai selecionar aquele checkbox.

select ‘Nome do item no Drop Down’, from: ‘nome do elemento drop down’ – Seleciona um item de um drop down.
ex: select ‘Apto’, from ‘tipo_moradia’

click_button ‘Cadastrar’ –Clica no botão cadastrar.

click_link ‘Home’ – Clica no link Home caso haja algum na página.

expect(page).to have_content ‘Cadastro efetuado com sucesso’ – Procura a mensagem e caso tenha, será sucesso.

within_frame 'id do frame' do
  comandos de para interagir dentro do frame
end - Entra no Frame para continuar os testes.

page.has_css?('css do que vc quer validar na tela', :text => 'texto pra ser validado', :visible => true) - Olha o texto que está no elemento e ve se esse texto está visível.

```
Uma ferramenta poderosa para busca de elementos HTML é o Firebug (eu gosto e prefiro) e nessa ferramenta você pode com a setinha clicar no elemento que você deseja e pegar a propriedade dele (ID por exemplo). No próximo eu vou falar como buscar elementos usando algum seletor. Mas por hora, se não tiver ID por exemplo, clica com botão direito na seleção do elemento e clica em copiar caminho CSS, vai em Console (no próprio Firebug e faça isso: $('aqui voce cola') e depois da enter pra ver se ele encontra o seu elemento.

Então vamos aplicar:

Todo teste automatizado é derivado de uma ação manual feita em algum momento em algum lugar, então vamos lá acessar o arquivo register.rb e deixar o nosso código da seguinte maneira:

```ruby
Given(/^I access the Walmart site$/) do
  visit "https://www.walmart.com.br/"
end

When(/^I click on Register$/) do
  find(:css, '#topbar-signup-link').click
end

And(/^fill in the fields$/) do
  within_frame 'iframeLogin' do
    fill_in 'notifymeClientName', with: 'Capybara for All'
    fill_in 'email', with: 'emailcapybaraforall@gmail.com'
    fill_in 'cpfcnpj', with: '02962965202'
    fill_in 'phone', with: '1124435654'
    fill_in 'password', with: 'inicial1234'
    fill_in 'signupClientPwdCheck', with: 'inicial1234'
    find(:css, '#signupButtonSend').click
  end
end

And(/^click Cadastar$/) do
  within_frame 'iframeLogin' do
    find(:css, '#signupButtonSend').click
  end
end

Then(/^my registration will be successfully made$/) do
  page.has_css?('input.suggestion-search', :text => 'Capybara for All', :visible => true)
end
```
Finalizando, ir no terminal, navegar até a página cucumber e executar o próprio comando cucumber e ver a mágica acontecer e quando terminar o teste, você verá o seguinte resultado:

```ruby
Feature: Register

  Scenario: Register on Wal Mart                   # features/specifications/register.feature:3
    Given I access the Walmart site                # features/step_definitions/register.rb:2
    When I click on Register                       # features/step_definitions/register.rb:6
    And fill in the fields                         # features/step_definitions/register.rb:10
    And click Cadastar                             # features/step_definitions/register.rb:22
    Then my registration will be successfully made # features/step_definitions/register.rb:26

1 scenario (1 passed)
5 steps (5 passed)
0m27.645s
```
Onde todos os passos foram executados em 28 segundos. Tudo passou =) ... Coisa linda demais.
