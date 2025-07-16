# Conectando o Azure Databricks com o PostgreSQL no Render
Tutorial passo a passo de como criar um workspace no Azure Databricks e conectá-lo a um banco de dados PostgreSQL hospedado no Render.
---

## ✅ Pré-requisitos

- Conta no [Azure](https://portal.azure.com/)
- Conta no [Render](https://render.com/)
- Banco de dados PostgreSQL já criado e ativo no Render

---

## 🧭 Etapas do tutorial

### 1. Acesse o Portal do Azure
- Acesse: [https://portal.azure.com](https://portal.azure.com)
- Faça login com sua conta Microsoft.

---

## 2. Criando o Workspace no Azure Databricks

<img width="1517" height="355" alt="azdatabricks" src="https://github.com/user-attachments/assets/3fe73abf-fb05-4bc8-8b6a-5ea38194affc" />

- No campo de busca do portal, digite **Azure Databricks**
- Clique no ícone conforme a imagem acima.
- Após o redirecionamento para uma nova tela, clique em **Criar**
- Na tela **Criar um workspace do Azure Databricks**, preencha os passos:
- **Assinatura**: Selecione sua assinatura ativa (ex: `Azure for Students` ou `Azure subscription 1`)
- **Grupo de recursos**: Selecione um existente (ex: `lab-databricks`) ou clique em **Criar novo**
- **Nome do Workspace**: Defina um nome exclusivo para o workspace (ex: `work-databricks`)
- **Região** (ex: West US 3)
- **Tipo de Preço**: Selecione a opção `Avaliação`
- **Nome do Grupo de Recursos Gerenciados**: deixe em branco (será preenchido automaticamente)
- Clique em **Revisar + Criar**
- Após a validação, clique em **Criar**

---

## 3. Acessando o Azure Databricks

<img width="1877" height="747" alt="workpace" src="https://github.com/user-attachments/assets/6da50ec1-07bb-4270-8d83-79be594434c7" />

- Após a implantação, clique em **Ir para o recurso**
- Clique em **Iniciar workspace**
- Você será redirecionado para o ambiente Databricks

---

## 4. Criando um notebook

<img width="1782" height="637" alt="notebooks" src="https://github.com/user-attachments/assets/a296c932-e652-44c9-8f0c-61b843041920" />

- No menu lateral cliquem em **+ Novo**
- Depois clique em **Notebook**
- Copie o código abaixo no notebook:
  
```python
import psycopg2

# Conexão com o banco
conn = psycopg2.connect(
    host="",
    database="",
    user="",
    password="",
    port=5432
)

cur = conn.cursor()

sql = input("Digite o comando SQL: ")

try:
    cur.execute(sql)

    # Se for uma consulta (SELECT), exibe os resultados
    if sql.strip().lower().startswith("select"):
        rows = cur.fetchall()
        for row in rows:
            print(row)
    else:
        # Para INSERT, UPDATE, DELETE
        conn.commit()
        print("Operação executada com sucesso!")

except Exception as e:
    print("Erro ao executar SQL:", e)

finally:
    cur.close()
    conn.close()
```

---

## 5. Coletando os dados do PostgreSQL no Render

<img width="1380" height="686" alt="render" src="https://github.com/user-attachments/assets/e93d7403-283e-40de-8abf-698861ed7f90" />

- Acesse seu banco PostgreSQL no [Render](https://dashboard.render.com/)
- Copie a **External Database URL** (ex: `postgresql://example_user:example_password@db-postgres-render.fake-region.render.com/example_database`)
> A partir dessa URL, extraia as seguintes informações para conexão:

| Parâmetro | Exemplo extraído                             |
|-----------|---------------------------------------------|
| host      | `db-postgres-render.fake-region.render.com` |
| database  | `example_database`                           |
| user      | `example_user`                              |
| password  | `example_password`                          |
| port      | `5432`        |


## 6. Executando o notebook

<img width="1783" height="737" alt="runn" src="https://github.com/user-attachments/assets/d14847ce-7053-4498-abdf-4f68b602cd91" />

- Altere o código com os dados da sua URL, siga o exemplo abaixo

```python
conn = psycopg2.connect(
    host="db-postgres-render.fake-region.render.com",
    database="example_database",
    user="example_user",
    password="example_password",
    port=5432
)
```
- Para executar o código clique em **Ligar**
- Selecione **Serveless**
- Clique em **Executar célula**
---

