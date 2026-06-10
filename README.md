Abaixo está um README completo que você pode usar como documentação do projeto do banco de dados do Adventure Biker BR. A estrutura foi pensada para um portal de motociclismo com cadastro de motos, motoclubes, eventos, notícias, roteiros, galerias e vídeos. O site tem foco na comunidade motociclista e conteúdo relacionado a viagens e motociclismo de aventura. ([Rota Biker][1])

# 🏍️ Adventure Biker BR - Aula Completa de Criação do Banco de Dados MySQL

## Objetivo

Criar um banco de dados robusto para o portal Adventure Biker BR capaz de armazenar:

* Motocicletas
* Marcas
* Motoclubes
* Eventos
* Notícias
* Roteiros de viagem
* Galerias de imagens
* Vídeos do YouTube
* Usuários
* Comentários
* Avaliações

---

# 1. Instalação do MySQL

## Ubuntu

```bash
sudo apt update
sudo apt install mysql-server
```

## Windows

Baixe:

[https://dev.mysql.com/downloads/installer/](https://dev.mysql.com/downloads/installer/)

Durante a instalação:

* Instale MySQL Server
* Instale MySQL Workbench
* Defina senha do usuário root

---

# 2. Criando o Banco

Acesse:

```sql
mysql -u root -p
```

Crie o banco:

```sql
CREATE DATABASE adventurebiker
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

Selecione o banco:

```sql
USE adventurebiker;
```

---

# 3. Tabela de Marcas

```sql
CREATE TABLE marcas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    slug VARCHAR(120) UNIQUE,
    logo VARCHAR(500),
    ativo BOOLEAN DEFAULT TRUE
);
```

Exemplo:

```sql
INSERT INTO marcas(nome,slug)
VALUES
('Honda','honda'),
('Yamaha','yamaha'),
('BMW','bmw');
```

---

# 4. Tabela de Motocicletas

```sql
CREATE TABLE motocicletas (

    id INT AUTO_INCREMENT PRIMARY KEY,

    marca_id INT NOT NULL,

    modelo VARCHAR(150) NOT NULL,

    versao VARCHAR(150),

    ano SMALLINT,

    categoria VARCHAR(100),

    cilindrada_cc DECIMAL(8,2),

    potencia_cv DECIMAL(8,2),

    torque_kgfm DECIMAL(8,2),

    peso_kg DECIMAL(8,2),

    tanque_litros DECIMAL(6,2),

    altura_assento_mm INT,

    combustivel VARCHAR(50),

    cambio VARCHAR(50),

    descricao_curta TEXT,

    descricao_completa LONGTEXT,

    youtube_url VARCHAR(500),

    imagem_destaque VARCHAR(500),

    slug VARCHAR(255),

    criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY (marca_id)
        REFERENCES marcas(id)
);
```

---

# 5. Galeria de Imagens

Cada moto pode possuir até 5 imagens.

```sql
CREATE TABLE motocicleta_galeria (

    id INT AUTO_INCREMENT PRIMARY KEY,

    motocicleta_id INT NOT NULL,

    imagem_url VARCHAR(500),

    ordem_exibicao TINYINT,

    FOREIGN KEY (motocicleta_id)
        REFERENCES motocicletas(id)
        ON DELETE CASCADE
);
```

---

# 6. Vídeos do YouTube

```sql
CREATE TABLE motocicleta_videos (

    id INT AUTO_INCREMENT PRIMARY KEY,

    motocicleta_id INT NOT NULL,

    titulo VARCHAR(255),

    youtube_url VARCHAR(500),

    FOREIGN KEY (motocicleta_id)
        REFERENCES motocicletas(id)
        ON DELETE CASCADE
);
```

---

# 7. Motoclubes

```sql
CREATE TABLE motoclubes (

    id INT AUTO_INCREMENT PRIMARY KEY,

    nome VARCHAR(255),

    cidade VARCHAR(150),

    estado CHAR(2),

    pais VARCHAR(100),

    descricao LONGTEXT,

    brasao VARCHAR(500),

    website VARCHAR(500),

    instagram VARCHAR(500),

    facebook VARCHAR(500),

    criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# 8. Eventos

```sql
CREATE TABLE eventos (

    id INT AUTO_INCREMENT PRIMARY KEY,

    titulo VARCHAR(255),

    descricao LONGTEXT,

    data_inicio DATETIME,

    data_fim DATETIME,

    cidade VARCHAR(150),

    estado CHAR(2),

    endereco VARCHAR(500),

    banner VARCHAR(500),

    criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# 9. Notícias

```sql
CREATE TABLE noticias (

    id INT AUTO_INCREMENT PRIMARY KEY,

    titulo VARCHAR(255),

    resumo TEXT,

    conteudo LONGTEXT,

    imagem VARCHAR(500),

    autor VARCHAR(150),

    publicado_em DATETIME,

    slug VARCHAR(255)
);
```

---

# 10. Roteiros de Viagem

```sql
CREATE TABLE roteiros (

    id INT AUTO_INCREMENT PRIMARY KEY,

    titulo VARCHAR(255),

    origem VARCHAR(255),

    destino VARCHAR(255),

    distancia_km DECIMAL(10,2),

    descricao LONGTEXT,

    mapa_url VARCHAR(500),

    imagem VARCHAR(500)
);
```

---

# 11. Usuários

```sql
CREATE TABLE usuarios (

    id INT AUTO_INCREMENT PRIMARY KEY,

    nome VARCHAR(150),

    email VARCHAR(255) UNIQUE,

    senha_hash VARCHAR(255),

    foto VARCHAR(500),

    criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# 12. Comentários

```sql
CREATE TABLE comentarios (

    id INT AUTO_INCREMENT PRIMARY KEY,

    usuario_id INT,

    noticia_id INT,

    comentario TEXT,

    criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY(usuario_id)
        REFERENCES usuarios(id),

    FOREIGN KEY(noticia_id)
        REFERENCES noticias(id)
);
```

---

# 13. Índices para SEO e Performance

```sql
CREATE INDEX idx_modelo
ON motocicletas(modelo);

CREATE INDEX idx_slug_moto
ON motocicletas(slug);

CREATE INDEX idx_evento_data
ON eventos(data_inicio);

CREATE INDEX idx_noticia_slug
ON noticias(slug);
```

---

# 14. Estrutura de Upload de Imagens

Recomendação:

```text
/uploads

    /motos
    /motoclubes
    /eventos
    /noticias
    /roteiros
```

Exemplo:

```text
/uploads/motos/yamaha-mt07-2025.jpg
```

---

# 15. Estrutura de URLs SEO

```text
/motos/yamaha/mt-07-2025

/motoclubes/jaquetas-negras

/eventos/encontro-nacional-harley-2026

/roteiros/serra-do-rio-do-rastro
```

---

# 16. Backup Automático

Linux:

```bash
mysqldump -u root -p adventurebiker > backup.sql
```

Restaurar:

```bash
mysql -u root -p adventurebiker < backup.sql
```

---

# 17. Próximos Passos

Após concluir este banco:

1. Criar API REST
2. Criar painel administrativo
3. Importar base FIPE
4. Importar motos automaticamente
5. Integrar Google Maps
6. Integrar YouTube
7. Criar sistema de avaliações
8. Criar ranking de motoclubes
9. Criar agenda de eventos
10. Criar aplicativo Android e iOS

---

# Resultado Final

O banco estará preparado para:

✅ Mais de 100.000 motocicletas

✅ Milhões de imagens

✅ Milhares de motoclubes

✅ Eventos nacionais

✅ Notícias

✅ Roteiros turísticos

✅ SEO avançado

✅ Integração futura com APIs FIPE

✅ Integração futura com aplicativos móveis

Esse README pode servir como base para um projeto profissional em PHP/Laravel, Node.js ou WordPress com banco MySQL.

[1]: https://www.monumentorotabiker.com.br/?utm_source=chatgpt.com "Rota Biker | Monumentos, Paradas e Rotas de Motociclismo no Brasil"
