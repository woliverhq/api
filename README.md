# Woliver REST API

<!-- TOC depthFrom:1 depthTo:2 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Introdução](#introdução)
- [Autenticação](#autenticação)
- [Leads](#leads)
- [Imóveis](#imóveis)
- [Visitas](#visitas)
- [Propostas](#propostas)
- [Recomendações](#recomendações)

<!-- /TOC -->

## Introdução

A Woliver é uma empresa dedicada a democratização ao acesso a tecnologias com foco em Real Estate.

Essa documentação possui o manual da API Woliver que pode ser utilizada por desenvolvedores para integrações com nossas tecnologias.

Todas as integrações são realizadas com uma imobiliária e a **BASE URL** da API segue o padrão:

```https
example.api.woliver.net
```

Referente a imobiliária de URL: [example.woliver.net](https://example.woliver.net). A propósito, para questões de documentação usaremos a imobiliária **example** nesse manual.

Esperamos que você goste da API Woliver, mas não hesite em [criar uma issue](https://github.com/woliverhq/api/issues/new) se houver algo faltando.

## Autenticação

Todos os chamados de API exigem uma API token. A API token deve ser requisitada para equipe Woliver e é única para cada imobiliária.

```http
GET /api/v1/example/?token=12345678901234567890123456789012
```

| Parâmetro | Tipo     | Descrição                              |
| :-------- | :------- | :------------------------------------- |
| `token`   | `string` | **Obrigatório**. Sua Woliver API token |

## Leads

### Criar lead

Esse endpoint é utilizado para a criação de novas leads. O usuário deverá prover os parâmetros listados a seguir.

```http
POST /api/v1/leads/?token=12345678901234567890123456789012
```

#### Query Parameters

| Parâmetro | Tipo     | Descrição                              |
| :-------- | :------- | :------------------------------------- |
| `token`   | `string` | **Obrigatório.** Sua Woliver API token |

#### Request

| Parâmetro      | Tipo     | Descrição                                                            |
| :------------- | :------- | :------------------------------------------------------------------- |
| `full_name`    | `string` | **Obrigatório.** Nome completo do lead                               |
| `cpf`          | `string` | CPF do lead (apenas números)                                         |
| `email`        | `string` | **Obrigatório.** E-mail do lead                                      |
| `phone_number` | `string` | Número de telefone do lead com DDD                                   |
| `listing`      | `number` | **Obrigatório.** Código do imóvel de acordo com o XML da imobiliária |
| `source`       | `slug`   | Slug name da origem da lead                                          |
| `source_id`    | `number` | Código da origem da lead                                             |
| `comments`     | `string` | Comentários do lead a respeito do imóvel                             |

```javascript
{
  "full_name": "Lead Woliver",
  "cpf": "11111111111",
  "email": "dev@woliver.com",
  "phone_number": "+554899999999",
  "listing": "75761",
  "source": "canaldigital",
  "source_id": 8,
  "comments": "Quero agendar uma visita!"
}
```

#### Response - 201 (application/json)

```javascript
{
    "id": 257,
    "user": {
        "id": 92,
        "phone_number": "+554899999999",
        "full_name": "Lead Woliver",
        "email": "dev@woliver.com"
    },
    "listing": "75761",
    "staff1": null,
    "source_id": 8
}
```

### Listar leads

Esse endpoint é utilizado para listar todas as leads existentes. O usuário deverá prover os parâmetros listados a seguir.

```http
GET /api/v1/leads/?token=12345678901234567890123456789012
```

#### Query Parameters

| Parâmetro | Tipo     | Descrição                              |
| :-------- | :------- | :------------------------------------- |
| `token`   | `string` | **Obrigatório.** Sua Woliver API token |

#### Response - 200 (application/json)

```javascript
[
  {
    id: 864,
    user: {
      id: 9,
      phone_number: "+5599999999999",
      full_name: "Lead Woliver",
      email: "dev@jungledevs.com",
    },
    listing: "75761",
    staff1: null,
    source_id: null,
  },
];
```

### Detalhes da lead

Esse endpoint é utilizado para acessar os detalhes de uma lead específica. O usuário deverá prover os parâmetros listados a seguir.

```http
GET /api/v1/leads/{id}/?token=12345678901234567890123456789012
```

#### Query Parameters

| Parâmetro | Tipo     | Descrição                              |
| :-------- | :------- | :------------------------------------- |
| `token`   | `string` | **Obrigatório.** Sua Woliver API token |

#### Request

| Parâmetro | Tipo     | Descrição                   |
| :-------- | :------- | :-------------------------- |
| `id`      | `string` | **Obrigatório**. Id da lead |

#### Response - 200 (application/json)

```javascript
{
    "id": 864,
    "user": {
        "id": 9,
        "phone_number": "+5599999999999",
        "full_name": "Lead Woliver",
        "email": "dev@jungledevs.com"
    },
    "listing": "75761",
    "staff1": null,
    "source_id": null
}
```

### Listar origem das leads

Esse endpoint é utilizado para acessar a lista cadastrada na Woliver de origem das leads

```http
GET /api/v1/leads-sources/?token=12345678901234567890123456789012
```

#### Response - 200 (application/json)

```javascript
[
    {
        "id": 1,
        "slug_name": "hints",
        "name": "hints.com.br"
    },
    {
        "id": 2,
        "slug_name": "ayla",
        "name": "Site Ayla"
    },
    {
        "id": 3,
        "slug_name": "olx",
        "name": "OLX"
    },
    {
        "id": 4,
        "slug_name": "mercadolivre",
        "name": "Mercado Livre"
    },
    {
        "id": 5,
        "slug_name": "chavenamao",
        "name": "Chave na mão"
    },
    {
        "id": 6,
        "slug_name": "imovelweb",
        "name": "Imovel Web"
    },
    {
        "id": 7,
        "slug_name": "zapimoveis",
        "name": "Zap Imóveis"
    }
    {
        "id": 8,
        "slug_name": "canaldigital",
        "name": "Canal Digital"
    },
    {
        "id": 9,
        "slug_name": "vivareal",
        "name": "Viva Real"
    },
     {
        "id": 10,
        "slug_name": "website",
        "name": "Website"
    },
    {
        "id": 11,
        "slug_name": "whatsapp",
        "name": "WhatsApp"
    },

]
```

#### Origem das leads cadastradas na Woliver

| Id  | Nome          | Slug         |
| :-- | :------------ | :----------- |
| 1   | hints.com.br  | hints        |
| 2   | Site Ayla     | ayla         |
| 3   | OLX           | olx          |
| 4   | Mercado Livre | mercadolivre |
| 5   | Chave na mão  | chavenamao   |
| 6   | Imovel Web    | imovelweb    |
| 7   | Zap Imóveis   | zapimoveis   |
| 8   | Canal Digital | canaldigital |
| 9   | Viva Real     | vivareal     |
| 10  | Website       | website      |
| 11  | WhatsApp      | whatsapp     |

## Imóveis

### Cadastrar novo imóvel

```http
POST /api/v1/backoffice-listings/
```

#### Request

| Parâmetro                 | Tipo      | Descrição                                                     |
| :------------------------ | :-------- | :------------------------------------------------------------ |
| `reference`               | `string`  | String de referência do imóvel, similar ao `external_id`      |
| `owner`                   | `number`  | Preço para aluguel do imóvel                                  |
| `staff`                   | `number`  | Id do usuário corretor responsável pelo registro do imóvel    |
| `url`                     | `string`  | Url do anúncio original do imóvel                             |
| `video`                   | `string`  | Url do YouTube do vídeo do imóvel                             |
| `exclusive`               | `boolean` | Imóvel exclusivo da imobiliária ou não                        |
| `new_release`             | `boolean` | Imóvel é lançamento ou não                                    |
| `sign_placed`             | `boolean` | Placa de divulgação colocada ou não                           |
| `vacancy`                 | `boolean` | Imóvel vago ou não                                            |
| `publicize_permission`    | `boolean` | Permissão para divulgação do imóvel                           |
| `advertisement_type`      | `string`  | Tipo de divulgação                                            |
| `registration_origin`     | `string`  | Origem do registro do imóvel: Back Office ou Canal Digital    |
| `registration_agency`     | `string`  | Agência na qual o imóvel foi registrado                       |
| `origin`                  | `number`  | Fetch de origem do imóvel                                     |
| `title`                   | `string`  | Título do anúncio                                             |
| `notes`                   | `string`  | Notas referentes ao imóvel                                    |
| `description`             | `string`  | Descrição do anúncio                                          |
| `listing_description`     | `string`  | Descrição do imóvel                                           |
| `condominium_description` | `string`  | Descrição do condomínio                                       |
| `neighborhood_description`| `string`  | Descrição do bairro                                           |
| `listing_type`            | `string`  | Tipo de imóvel: Casa e/ou Apartamento                         |
| `purpose_type`            | `string`  | Propósito do imóvel: Comercial/Residencial/Ambos              |
| `transaction_type`        | `string`  | Tipo de negociação: Aluguel/Venda/Ambos                       |
| `floor`                   | `number`  | Andar do imóvel                                               |
| `bedrooms`                | `number`  | Número de quartos                                             |
| `bathrooms`               | `number`  | Número de banheiros                                           |
| `number_of_ensuites`      | `number`  | Número de suítes                                              |
| `parking_spots`           | `number`  | Número de vagas de garagem                                    |
| `covered_parking_spots`   | `number`  | Número de vagas de garagem cobertas                           |
| `parking_spots_number`    | `array`   | Número das vagas de garagem                                   |
| `garage_type`             | `string`  | Tipo de garagem                                               |
| `hobbybox`                | `boolean` | Possui hobbybox ou não                                        |
| `hobbybox_number`         | `string`  | Número do hobbybox                                            |
| `living_area`             | `number`  | Área útil                                                     |
| `total_area`              | `number`  | Área total                                                    |
| `furnished`               | `string`  | Mobília do imóvel: Mobiliado/Semi-mobiliado/Não mobiliado     |
| `pets`                    | `boolean` | Aceita animais de estimação ou não                            |
| `penthouse`               | `boolean` | É cobertura ou não                                            |
| `solar_orientation`       | `string`  | Orientação solar: Manhã/Tarde/Ambos/Desconhecido              |
| `building_position`       | `string`  | Posição em relação à fachada do edifício: Frente/Lado/Fundos  |
| `building_name`           | `string`  | Nome do edifício                                              |
| `concierge_type`          | `string`  | Tipo de portaria                                              |
| `key_type`                | `string`  | Tipo de chave                                                 |
| `address`                 | `string`  | Endereço                                                      |
| `extra_address_info`      | `string`  | Complemento                                                   |
| `street_number`           | `string`  | Número da rua                                                 |
| `coordinates`             | `string`  | Coordenadas geográficas                                       |
| `postal_code`             | `string`  | CEP                                                           |
| `neighborhood`            | `string`  | Bairro                                                        |
| `city`                    | `string`  | Cidade                                                        |
| `neighborhood_slug`       | `string`  | Slug name do bairro                                           |
| `city_slug`               | `string`  | Slug name da cidade                                           |
| `state`                   | `string`  | UF do imóvel                                                  |
| `rent_price`              | `number`  | Valor do aluguel do imóvel                                    |  
| `sale_price`              | `number`  | Valor de venda do imóvel                                      |
| `condominium_fee`         | `number`  | Taxa de condomínio                                            |
| `taxes_type`              | `string`  | Tipo de pagamento do IPTU: Anual/Mensal                       |
| `taxes`                   | `number`  | Valor do IPTU                                                 |
| `insurance`               | `number`  | Valor do seguro do imóvel                                     |
| `garbage_fee`             | `number`  | Taxa de coleta de lixo                                        |
| `available`               | `boolean` | Imóvel disponível ou não                                      |
| `invalid`                 | `boolean` | Informações do imóvel inválidas ou não                        |

### Atualizar imóvel
O endpoint possibilita alterar dados de um imóvel já cadastrado na plataforma.

```http
PATCH /api/v1/backoffice-listings/{external_id}/
```

#### Request

| Parâmetro                 | Tipo      | Descrição                                                     |
| :------------------------ | :-------- | :------------------------------------------------------------ |
| `reference`               | `string`  | String de referência do imóvel, similar ao `external_id`      |
| `owner`                   | `number`  | Preço para aluguel do imóvel                                  |
| `staff`                   | `number`  | Id do usuário corretor responsável pelo registro do imóvel    |
| `url`                     | `string`  | Url do anúncio original do imóvel                             |
| `video`                   | `string`  | Url do YouTube do vídeo do imóvel                             |
| `exclusive`               | `boolean` | Imóvel exclusivo da imobiliária ou não                        |
| `new_release`             | `boolean` | Imóvel é lançamento ou não                                    |
| `sign_placed`             | `boolean` | Placa de divulgação colocada ou não                           |
| `vacancy`                 | `boolean` | Imóvel vago ou não                                            |
| `publicize_permission`    | `boolean` | Permissão para divulgação do imóvel                           |
| `advertisement_type`      | `string`  | Tipo de divulgação                                            |
| `registration_origin`     | `string`  | Origem do registro do imóvel: Back Office ou Canal Digital    |
| `registration_agency`     | `string`  | Agência na qual o imóvel foi registrado                       |
| `origin`                  | `number`  | Fetch de origem do imóvel                                     |
| `title`                   | `string`  | Título do anúncio                                             |
| `notes`                   | `string`  | Notas referentes ao imóvel                                    |
| `description`             | `string`  | Descrição do anúncio                                          |
| `listing_description`     | `string`  | Descrição do imóvel                                           |
| `condominium_description` | `string`  | Descrição do condomínio                                       |
| `neighborhood_description`| `string`  | Descrição do bairro                                           |
| `listing_type`            | `string`  | Tipo de imóvel: Casa e/ou Apartamento                         |
| `purpose_type`            | `string`  | Propósito do imóvel: Comercial/Residencial/Ambos              |
| `transaction_type`        | `string`  | Tipo de negociação: Aluguel/Venda/Ambos                       |
| `floor`                   | `number`  | Andar do imóvel                                               |
| `bedrooms`                | `number`  | Número de quartos                                             |
| `bathrooms`               | `number`  | Número de banheiros                                           |
| `number_of_ensuites`      | `number`  | Número de suítes                                              |
| `parking_spots`           | `number`  | Número de vagas de garagem                                    |
| `covered_parking_spots`   | `number`  | Número de vagas de garagem cobertas                           |
| `parking_spots_number`    | `array`   | Número das vagas de garagem                                   |
| `garage_type`             | `string`  | Tipo de garagem                                               |
| `hobbybox`                | `boolean` | Possui hobbybox ou não                                        |
| `hobbybox_number`         | `string`  | Número do hobbybox                                            |
| `living_area`             | `number`  | Área útil                                                     |
| `total_area`              | `number`  | Área total                                                    |
| `furnished`               | `string`  | Mobília do imóvel: Mobiliado/Semi-mobiliado/Não mobiliado     |
| `pets`                    | `boolean` | Aceita animais de estimação ou não                            |
| `penthouse`               | `boolean` | É cobertura ou não                                            |
| `solar_orientation`       | `string`  | Orientação solar: Manhã/Tarde/Ambos/Desconhecido              |
| `building_position`       | `string`  | Posição em relação à fachada do edifício: Frente/Lado/Fundos  |
| `building_name`           | `string`  | Nome do edifício                                              |
| `concierge_type`          | `string`  | Tipo de portaria                                              |
| `key_type`                | `string`  | Tipo de chave                                                 |
| `address`                 | `string`  | Endereço                                                      |
| `extra_address_info`      | `string`  | Complemento                                                   |
| `street_number`           | `string`  | Número da rua                                                 |
| `coordinates`             | `string`  | Coordenadas geográficas                                       |
| `postal_code`             | `string`  | CEP                                                           |
| `neighborhood`            | `string`  | Bairro                                                        |
| `city`                    | `string`  | Cidade                                                        |
| `neighborhood_slug`       | `string`  | Slug name do bairro                                           |
| `city_slug`               | `string`  | Slug name da cidade                                           |
| `state`                   | `string`  | UF do imóvel                                                  |
| `rent_price`              | `number`  | Valor do aluguel do imóvel                                    |  
| `sale_price`              | `number`  | Valor de venda do imóvel                                      |
| `condominium_fee`         | `number`  | Taxa de condomínio                                            |
| `taxes_type`              | `string`  | Tipo de pagamento do IPTU: Anual/Mensal                       |
| `taxes`                   | `number`  | Valor do IPTU                                                 |
| `insurance`               | `number`  | Valor do seguro do imóvel                                     |
| `garbage_fee`             | `number`  | Taxa de coleta de lixo                                        |
| `available`               | `boolean` | Imóvel disponível ou não                                      |
| `invalid`                 | `boolean` | Informações do imóvel inválidas ou não                        |


### Listar imóveis

Esse endpoint é utilizado para obter a lista de imóveis disponíves da imobiliária.

```http
GET /api/v1/listings/?token=12345678901234567890123456789012
```

#### Query Parameters

| Parâmetro        | Tipo      | Descrição                                                     |
| :--------------- | :-------- | :------------------------------------------------------------ |
| `token`          | `string`  | **Obrigatório**. Sua Woliver API token                        |
| `price`          | `string`  | Preço para aluguel do imóvel                                  |
| `taxes_included` | `boolean` | Considerar valores de IPTU e condomínio no valor de aluguel   |
| `city`           | `string`  | Cidades para busca de imóveis                                 |
| `neighborhoods`  | `string`  | Bairros para busca de imóveis                                 |
| `bathrooms`      | `number`  | Número de banheiros do imóvel                                 |
| `bedrooms`       | `number`  | Número de quartos do imóvel                                   |
| `parking_spots`  | `number`  | Número de garagens do imóvel                                  |
| `furnished`      | `string`  | Tipo de mobilia: "furnished", "semi_furnished", "unfurnished" |
| `pets`           | `boolean` | Imóvel permite pets ou não                                    |
| `listing_type`   | `string`  | Tipo de imóvel: Casa e/ou Apartamento                         |

Os parâmetros opcionais, quando utilizados, serão levados em conta na atribuição de `scores` aos imóveis, gerando uma
pesquisa personalizada.

#### Response - 200 (application/json)

```javascript
{
    "count": 667,
    "next": "https://example.api.woliver.net/api/v1/listings/",
    "previous": null,
    "exact": 122,
    "results": [
        {
            "id": 894,
            "liked": null,
            "disliked": null,
            "score": 100.0,
            "exact": true,
            "distance": null,
            "booking": null,
            "coordinates": {
                "type": "Point",
                "coordinates": []
            },
            "converted_pictures": [
                {
                    "picture": "https://picture-url.jpg",
                    "thumbnail": "https://thumbnail-url.jpg",
                    "picture_800x600": "picture-url-800x600.jpg"
                },
            ],
            "pictures": [
                "https://picture-1-url.jpg",
            ],
            "reserved": false,
            "external_id": "00000",
            "title": "TÍTULO DO ANÚNCIO",
            "url": "https://www.example.com.br/codigo/16430",
            "video": null,
            "address": "RUA",
            "extra_address_info": null,
            "street_number": "000",
            "postal_code": "00000-000",
            "neighborhood": "BAIRRO",
            "city": "CIDADE",
            "neighborhood_slug": "bairro",
            "city_slug": "cidade",
            "state": "UF",
            "listing_type": "res_apartment",
            "purpose_type": "residential",
            "transaction_type": "rent",
            "bedrooms": 3,
            "bathrooms": 1.0,
            "number_of_ensuites": 0,
            "parking_spots": 1,
            "living_area": 75.0,
            "total_area": 84.0,
            "furnished": "unfurnished",
            "description": "Descrição do imóvel",
            "listing_description": null,
            "condominium_description": null,
            "rent_price": 1800.0,
            "sale_price": null,
            "condominium_fee": 586.0,
            "taxes": 97.0,
            "insurance": null,
            "available": true,
            "pets": false,
            "amenities": null,
            "incomplete_email_sent": false,
            "invalid": false,
            "contract_signed": false,
            "zone": 5,
            "origin": 17,
            "origin_name": "Brognoli",
        },
    ],
    "hide_filters": false,
    "filters": {},
    "name": null
}
```

## Visitas

### URL para agendamento de visita

Para direcionar o usuário para o agendamento de visita para um imóvel específico, basta utilizar a URL com o seguinte formato:

```https
example.woliver.net/agendamento/ID_DO_IMOVEL
```

Exemplo, para o imóvel com código 74802 no demo.woliver.net, você pode agendar uma visita acessando https://demo.woliver.net/agendamento/74802.

Essa URL pode ser utilizada no próprio site da imobiliária ou quaisquer ferramentas com a função de direcionar o usuário para o agendamento de visita em um imóvel específico. Por exemplo, adicionando um botão "Agendar Visita" nos detalhes do imóvel no site da imobiliária em si.

### URL para agendamento de visita com identificador da origem

Se você busca entender qual a origem do agendamento de visita em um imóvel, basta adicionar a source_id ou slug na URL de agendamento de visita:

- Opção 1 (source id): https://demo.woliver.net/agendamento/29939?source_id=7

- Opção 2 (source slug): https://demo.woliver.net/agendamento/29939?source=zapimoveis

Os valores possíveis para o campo source podem ser visualizados na tabela acima ([Origem das leads cadastradas na Woliver](#origem-das-leads-cadastradas-na-woliver)). Esses valores serão exportados junto com as informações da lead no arquivo dados.csv que pode ser acessado no back office da imobiliária.

### Criar novo agendamento

Esse endpoint é utilizado para criar um agendamento de visita a um imóvel.

```http
POST /api/v1/bookings/?token=12345678901234567890123456789012
```

#### Query Parameters

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `token` | `string` | **Obrigatório**. Sua Woliver API token |

#### Request

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `listing` | `number` | **Obrigatório**. Código do imóvel de acordo com o XML da imobiliária |
| `date_and_time` | `datetime` | **Obrigatório** Data e horário da visita |
| `user_id` | `number` | **Obrigatório** Código do cliente que deseja agendar a visita |
| `staff_hash` | `string` | Código do corretor responsável pelo agendamento da visita |


```javascript
{
	"listing": "00000",
	"date_and_time": "2020-12-25T15:00:00Z",
	"user_id": 2,
	"staff_hash": "staff"
}
```

#### Response - 201 (application/json)

```javascript
{
    "id": 12,
    "user": {
        "id": 2,
        "phone_number": "+5599999999999",
        "email": "dev@woliver.com",
        "full_name": "Lead Wolier",
        "cpf": "00000000000",
        "date_of_birth": "1996-12-03",
        "rg": "0000000",
        "issuing_agency": "SSP",
        "gender": "",
        "civil_status": null,
        "nationality": "null,
        "occupation": null,
        "professional_data": null,
        "current_address": "Address",
        "role": "client",
        "backoffice_filters": null,
        "need_change_password": false,
        "picture": null,
        "active_lease": null
    },
    "staff": {
        "id": 5,
        "phone_number": null,
        "email": "",
        "full_name": "Staff",
        "cpf": null,
        "date_of_birth": null,
        "rg": null,
        "issuing_agency": null,
        "gender": null,
        "civil_status": null,
        "nationality": null,
        "occupation": null,
        "professional_data": null,
        "current_address": null,
        "role": "staff",
        "backoffice_filters": null,
        "need_change_password": false,
        "picture": null,
        "active_lease": null
    },
    "lease": {
        "id": 2,
        "rent_price": 10000.0,
        "total_price": 10858.0,
        "status": "booking_scheduled",
        "monthly_income": null,
        "total_monthly_income": 0,
        "created_at": "2020-07-09T20:45:06.520186Z",
        "updated_at": "2020-07-09T21:23:32.599802Z",
        "income_approved": null,
        "identification_approved": null,
        "proof_of_income_approved": null,
        "credit_card_statement_approved": null,
        "required_income": 54290.0,
        "proposal": null,
        "warranty": "rapid",
        "fire_insurance_price": null,
        "fire_insurance_agreed": null
    },
    "phone_number": "+5599999999999",
    "available": true,
    "created_at": "2020-07-31T20:43:26.011032Z",
    "updated_at": "2020-07-31T20:43:26.011101Z",
    "date_and_time": "2020-12-25T15:00:00Z",
    "warned": false,
    "cancelled": false,
    "booking_extra_info": null,
    "booking_key_option": null,
    "listing": {
        "id": 4,
        "external_id": 00000,
        "title": null,
        "address": "RUA",
        "street_number": "0",
        "neighborhood": "BAIRRO",
        "city": "CIDADE",
        "converted_pictures": [],
        "rent_price": 10000.0,
        "condominium_fee": 580.0,
        "taxes": 278.0,
        "insurance": null,
        "total_price": 10858.0,
        "bedrooms": 1,
        "listing_type": "res_home",
        "interested_people": 0,
        "reserved": false,
        "contract_signed": false
    }
}
```

## Propostas

### Listar Propostas

Esse endpoint é utilizado para listar todas as propostas da imobiliára.

```http
GET /api/v1/proposals/?token=12345678901234567890123456789012
```

#### Query Parameters

| Parâmetro | Tipo     | Descrição                              |
| :-------- | :------- | :------------------------------------- |
| `token`   | `string` | **Obrigatório**. Sua Woliver API token |

#### Response - 200 (application/json)

```javascript
[
  {
    id: 2,
    booking: 2,
    living_type: "family",
    number_of_people: 1,
    has_pet: false,
    pet_description: "",
    self_description: "",
    rent_price: 1450.0,
    request_description: null,
    approved: null,
    created_at: "2020-07-27T17:04:27.118717Z",
    active: true,
    documents: [],
    lead: {
      id: 6,
      user: {
        id: 11,
        phone_number: null,
        email: "dev@woliver.com",
        full_name: "Lead Woliver",
      },
      leads_source: null,
      status: "not_initiated",
    },
    external_id: "0",
    listing: {
      external_id: "0",
      thumbnail: null,
      rent_price: 1450.0,
      total_price: 1450.0,
      address: "RUA",
      neighborhood: "BAIRRO",
    },
    from_staff: false,
    suggested_price: null,
    owner_price: null,
    price_requests: [],
    modification_requests: [],
  },
];
```

## Recomendações

### Criar URL de recomendações - filtros de busca

Esse endpoint é utilizado para a geração de uma URL com recomendações de imóveis a partir de filtros de busca. Ao abrir a URL a lead irá visualizar uma lista de imóveis organizados de acordo com os parâmetros de criação listados a seguir.

```http
POST /api/v1/recommendations/?token=12345678901234567890123456789012

```

#### Query Parameters

| Parâmetro | Tipo     | Descrição                              |
| :-------- | :------- | :------------------------------------- |
| `token`   | `string` | **Obrigatório**. Sua Woliver API token |

#### Request

| Parâmetro        | Tipo      | Descrição                                                     |
| :--------------- | :-------- | :------------------------------------------------------------ |
| `coordinates`    | `string`  | Coordenadas para buscar imóveis próximos                      |
| `price`          | `string`  | Preço para aluguel do imóvel                                  |
| `taxes_included` | `boolean` | Considerar valores de IPTU e condomínio no valor de aluguel   |
| `city`           | `string`  | Cidades para busca de imóveis                                 |
| `neighborhoods`  | `string`  | Bairros para busca de imóveis                                 |
| `bathrooms`      | `number`  | Número de banheiros do imóvel                                 |
| `bedrooms`       | `number`  | Número de quartos do imóvel                                   |
| `parking_spots`  | `number`  | Número de garagens do imóvel                                  |
| `furnished`      | `string`  | Tipo de mobilia: "furnished", "semi_furnished", "unfurnished" |
| `pets`           | `boolean` | Imóvel permite pets ou não                                    |
| `listing_type`   | `string`  | Tipo de imóvel: Casa e/ou Apartamento                         |
| `name`           | `string`  | Nome da lead                                                  |
| `email`          | `string`  | Email da lead utilizado para enviar recomendações             |
| `staff_email`    | `string`  | Email do corretor responsável pela recomendação               |
| `hide_filters`   | `boolean` | Esconder os filtros no header da url de recomendações         |
| `limit`          | `number`  | Limite de imóveis a serem retornados                          |
| `page_size`      | `number`  | Número de imóveis a serem retornados por página               |

```javascript
{
  "coordinates": "[[-27.570903, -48.508389]]",
  "price": 1500.0,
  "city": "florianopolis,sao-jose",
  "neighborhoods": "trindade,itacorubi",
  "bathrooms": 1,
  "bedrooms": 2,
  "parking_spots": 1,
  "furnished": "unfurnished",
  "pets": "true",
  "listing_type": "res_home,res_apartment",
  "nome": "",
  "email": "dev@woliver.com.br",
  "staff_email": "staff@woliver.com.br",
  "hide_filters": false,
  "limit": 10,
  "page_size": 3
}
```

#### Response - 201 (application/json)

```javascript
{
  "url": "https://example.woliver.net/recomendacoes/k28Jv5?staff_hash=STAFF"
}
```

### Criar URL de recomendações - código do imóvel

Esse endpoint é utilizado para a geração de uma URL com recomendações de imóveis a partir de um código do imóvel. Ao abrir a URL a lead irá visualizar uma lista de imóveis similares ao imóvel referente ao código passado como parâmetro na url.

```http
POST /api/v1/listings/{listing_id}/recommend/?token=12345678901234567890123456789012
```

#### Query Parameters

| Parâmetro | Tipo     | Descrição                              |
| :-------- | :------- | :------------------------------------- |
| `token`   | `string` | **Obrigatório**. Sua Woliver API token |

#### Request

| Parâmetro      | Tipo     | Descrição                                                            |
| :------------- | :------- | :------------------------------------------------------------------- |
| `listing_id`   | `string` | **Obrigatório**. Código do imóvel de acordo com o XML da imobiliária |
| `phone_number` | `string` | Telefone da lead                                                     |
| `name`         | `string` | Nome da lead                                                         |
| `email`        | `string` | Email da lead utilizado para enviar recomendações                    |

```javascript
{
    "phone_number": "+5548999999999"
    "name": "Lead Woliver",
    "email": "dev@woliver.com.br"
}
```

#### Response - 201 (application/json)

```javascript
}
  "url": "https://example.woliver.net/recomendacoes/k28Jv5?staff_hash=STAFF"
}
```

