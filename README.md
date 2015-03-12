# aula-jsf

#Criando o primeiro JSF Básico

1. Crie uma página simples public/pages/index.xhtml como a conteúdo descrito abaixo:

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
	xmlns:ui="http://java.sun.com/jsf/facelets"
	xmlns:f="http://java.sun.com/jsf/core"
	xmlns:h="http://java.sun.com/jsf/html"
	xmlns:p="http://primefaces.org/ui">
<body>
	<h1>Minha primeira página JSF</h1>
	<h:outputText value="#{meuBean.ola}" />
</body>
</html>
```

2. Crie a classe br.edu.ifrs.canoas.tads.lds.control.mb.MeuBean para gerar a saída de dados (outputText).

```java
@ManagedBean
public class MeuBean {
	
	private String ola = "OLAAAA!!!";

	public String getOla() {
		return ola;
	}

	public void setOla(String ola) {
		this.ola = ola;
	} 
}
```

#Criando um CRUD básico

1. Crie uma classe POJO br.edu.ifrs.canoas.tads.lds.bean.Usuario com "String email" e "String senha".

2. Crie um ManagedBean br.edu.ifrs.canoas.tads.lds.control.mb.ManterUsuarioMB com "Usuario usuario" e "List<Usuario> usuarios". Neste bean, além de @ManagedBean, coloque a anotação @SessionScoped (esse bean será mantido enquanto o usuário não fechar a seção com o servidor - ou o navegador)

```java
package br.edu.ifrs.canoas.tads.lds.control.mb;

import java.io.Serializable;
import java.util.ArrayList;
import java.util.List;

import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;

import br.edu.ifrs.canoas.tads.lds.bean.Usuario;

@SessionScoped
@ManagedBean
public class ManterUsuarioMB implements Serializable {

	private static final long serialVersionUID = -6537292603552968031L;

	private Usuario usuario;
	private List<Usuario> usuarios;
	
	public ManterUsuarioMB() {
		this.usuario = new Usuario();
		this.usuarios = new ArrayList<Usuario>();
	}
	
	// getters e setters
}
```
3. Crie o seguinte arquivo private/pages/cadastroUsuarios.xhtml

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
	xmlns:ui="http://java.sun.com/jsf/facelets"
	xmlns:f="http://java.sun.com/jsf/core"
	xmlns:h="http://java.sun.com/jsf/html"
	xmlns:p="http://primefaces.org/ui">

<h:head>
	<title>Cadastrar Usuário</title>
</h:head>

<h:body>
	<h1>
		<h:outputLabel styleClass="title" value="Cadastrar Usuário" />
	</h1>
	<h:form>
		<p:messages id="msgs" />

		<p:panel id="panel" header="Novo Usuário">

			<h:panelGrid columns="2" cellpadding="5">

				<p:outputLabel for="email" value="E-mail:" />
				<p:inputText id="email" value="#{manterUsuarioMB.usuario.email}"
					required="true" label="E-mail"/>

				  
				<p:outputLabel for="senha" value="Senha:" />
				<p:inputText id="senha" value="#{manterUsuarioMB.usuario.senha}"
					label="Senha" >
				</p:inputText>

			</h:panelGrid>
		</p:panel>

		<p:commandButton value="Salva" action="#{manterUsuarioMB.salva}" ajax="false" />
		 
	</h:form>
</h:body>
</html>
```
4. Adeque o código do MB para que funcione conforme as instruções do xhtml.


5. Adicione uma usando <p:dataTable>. Procure em http://www.primefaces.org/showcase/ui/data/datatable/basic.xhtml
Nesta tabela, você deve apresentar usuario e senha.


6. Agora vamos popular esta tabela com os dados do formulário. Para isso, atualize o método salva da classe br.edu.ifrs.canoas.tads.lds.control.mb.ManterUsuarioMB conforme código abaixo:

```java
	public void salva(){
		if (!usuarios.contains(usuario)){
			usuarios.add(usuario);
			usuario = new Usuario();
		}
	} 	
```

7. Altere a anotação @SessionScope para @RequestScope, reinicie o JBoss e note que os dados não estão mais sendo mantidos na seção, mas apenas na requisição. Preencha o formulário e submeta, veja que os dados foram mantidos apenas na solicitação para o servidor e as solicitações anteriores foram perdidas. Agora altere o escopo para  @ViewScope, reinicie o JBoss e veja o que acontece (os dados são mantidos apenas enquanto está acessando a página).Altere para @ApplicationScope e veja a diferença.

8. Inclua as operações abaixo na tabela e implemente as ações correspondentes no ManagedBean.
```html
			<p:column headerText="Ações">
				<h:commandLink value="Editar">
					<f:setPropertyActionListener value="#{usr}" 
						target="#{manterUsuarioMB.usuario}" />
				</h:commandLink>
				 | 
				 <h:commandLink value="Excluir" action="#{manterUsuarioMB.exclui}">
					<f:setPropertyActionListener value="#{usr}" 
						target="#{manterUsuarioMB.usuario}" />
				</h:commandLink>
			</p:column>
```
