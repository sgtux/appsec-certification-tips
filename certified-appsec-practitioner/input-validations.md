# Mecanismos de validação de entrada.

A validação de entrada (Input Validation) é uma prática essencial para garantir que dados fornecidos por usuários ou sistemas externos sejam válidos, seguros e adequados para processamento. Aqui estão exemplos de mecanismos de validação de entrada divididos por tipo:

## 1. Validação no Lado do Servidor

A validação no lado do servidor é executada após os dados serem enviados à aplicação. É crucial, mesmo que você também valide no cliente, pois os dados podem ser manipulados antes de chegar ao servidor.


```csharp
    // Exemplo de validação de e-mail no .NET com RegEx
    var emailRegex = new Regex(@"^[^@\s]+@[^@\s]+\.[^@\s]+$");
    if (!emailRegex.IsMatch(inputEmail)) {
        throw new Exception("E-mail inválido!");
    }
```

Utilização de bibliotecas de validação:
```js
    // Frameworks como FluentValidation (.NET), Joi (Node.js) ou Bean Validation (Java) permitem criar regras de validação robustas.

    const Joi = require('joi');
    const schema = Joi.object({
        username: Joi.string().alphanum().min(3).max(30).required(),
        password: Joi.string().pattern(new RegExp('^[a-zA-Z0-9]{3,30}$')),
    });
    const { error } = schema.validate({ username: 'user1', password: 'pass' });
```

Validação de tipos e comprimentos para garantir que valores estejam no tipo e tamanho corretos.

```js
if (input.length() > 255) {
    throw new IllegalArgumentException("Entrada muito longa!");
}
```

## 2. Validação no Lado do Cliente

Essa validação melhora a experiência do usuário, mas nunca deve ser a única linha de defesa. 

**Validações de entrada sempre devem ser aplicadas no lado do servidor!**
```html
<!-- Atributos HTML5:
    O HTML5 fornece atributos como required, maxlength, pattern, entre outros. -->

<input type="text" name="username" required pattern="[A-Za-z0-9]{3,20}" title="3-20 caracteres alfanuméricos">
```

Validação com JavaScript:
```js
// Scripts customizados para validar antes do envio do formulário.

const input = document.getElementById('username');
if (!/^[A-Za-z0-9]{3,20}$/.test(input.value)) {
    alert('Nome de usuário inválido!');
}
```

## 3. Validação Baseada em Contexto

Diferentes contextos podem exigir diferentes tipos de validação.
```python
# Números e valores numéricos:
# Validação de intervalos.

if not (0 <= value <= 100):
    raise ValueError("O valor deve estar entre 0 e 100")
```

Datas e Horários:
```php
// Garantir que valores de datas sejam válidos.

if (!strtotime($inputDate)) {
    die("Data inválida");
}
```

Identificadores e IDs:
```js
// Validação de UUIDs ou identificadores únicos.

const isValidUUID = /^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/.test(uuid);
if (!isValidUUID) {
    console.error('UUID inválido');
}
```
4. Validação de Dados Estruturados
```js
// Para entradas JSON, XML, ou outros formatos estruturados.

// JSON Schema Validation:
// Usado para garantir que o JSON segue um esquema.

const Ajv = require('ajv');
const ajv = new Ajv();
const schema = {
    type: 'object',
    properties: {
        name: { type: 'string' },
        age: { type: 'integer', minimum: 0 }
    },
    required: ['name', 'age']
}
const validate = ajv.compile(schema)
const valid = validate({ name: 'John', age: 30 })
if (!valid) 
    console.log(validate.errors)
```

Validação de XML com XSD:
```java
// Garantir que o XML segue um esquema definido.

SchemaFactory factory = SchemaFactory.newInstance(XMLConstants.W3C_XML_SCHEMA_NS_URI);
Schema schema = factory.newSchema(new File("schema.xsd"));
Validator validator = schema.newValidator();
validator.validate(new StreamSource(new File("file.xml")));
```

## 5. Sanitização como Parte da Validação

Certifique-se de que os dados não só são válidos, mas também seguros.
```php
// Remover Tags de HTML para evitar ataques XSS.

$sanitizedInput = htmlspecialchars($input, ENT_QUOTES, 'UTF-8');

// Whitelist de caracteres para permitir apenas caracteres esperados.

const sanitized = input.replace(/[^a-zA-Z0-9]/g, '');

// Remoção de SQL Injection utilizando Prepared Statements em vez de concatenar strings.

SELECT * FROM users WHERE username = ?
```

Esses mecanismos cobrem diferentes níveis de validação e garantem a segurança e a robustez da aplicação em vários contextos. Sempre combine validações para minimizar riscos!