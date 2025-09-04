# CREATE A NEW VIEW IN FULLWMS

## KEY TABLES
 - ger_modulo
 - ger_modulo_permissao
 - ger_menus
 - ger_modulo_tipo_permissao

## PHASE 1: CREATE ABSTRACTION VIEW IN THE DATABASE
Before, create a file with every command that you use, and first use it in a controlled development environment.
- **STEP 1: Insert the new module in the main table**<br/>
    This is an example of the query:
    ```sql
        INSERT INTO
            ger_modulo (
              ID,
              NOME,
              DISPONIVEL,
              TABELA,
              ENDPOINT,
              MODULO_PAI_ID,
              URL,
              MENU,
              TIPO,
              NOME_EN,
              NOME_SP
          )
        VALUES (
            110230,
            'Paripassu - Recebimentos no Sincronizados',
            'S',
            '',
            '/api/re-sync',
            '',
            '/re-sync',
            'N',
            NULL,
            'Paripassu - Unsynchronized Receipts',
            'Paripassu - Recibos no sincronizados'
        )
    ```

- **STEP 2: Assign basic permissions**<br/>
    This is an example of the query using the module *ID 110230*:
    ```sql
         INSERT INTO ger_modulo_permissao (IDMODULO, Idtipopermissao) VALUES (110230, 2);
    ```
    The allowed permissions are:
    - 1: Create
    - 2: Edit
    - 3: Show
    - 4: Delete
    - 5: Autocomplete (not implemented)
    - 6: Action
    - 7: Readonly
    - 8: Form

    These permissions are available in the table `ger_modulo_tipo_permissao`

- **STEP 3 (Optional): Add the module for action**<br/>
    This is an example of the query for creation of action of modules, when the module create need execute action in database:
    ```sql
        INSERT INTO
            ger_modulo (
              ID,
              NOME,
              DISPONIVEL,
              TABELA,
              ENDPOINT,
              MODULO_PAI_ID,
              URL,
              MENU,
              TIPO,
              NOME_EN,
              NOME_SP
          )
        VALUES (
            1000000XXX,
            'Action name',
            'S',
            '',
            '/api/{service_name}/action/{action_name}',
            '',
            '',
            'N',
            'A',
            '',
            ''
        )
    ```
    When the module of action is create, automatic is create the permition for this.

- **STEP 4 (Optional): Add the module to the main menu**
    ```sql
        INSERT INTO
            ger_menus (
              ATIVO,
              MNU_FORMAABERTURA,
              GER_MENU_ID,
              EMPRESA,
              GER_SISTEMA_ID,
              DESCRICAO,
              FORM,
              GER_MENUPAI_ID,
              ORDEM,
              FORMAABERTURA,
              FORM_WEB,
              ATIVO_WEB
            )
        VALUES
            (
              'S',
              'F',
              YYYXXX, -- YYY: id of parent, XXX: id for your module
              1,
              3,
              'Module name',
              NULL,
              110, -- YYY: id of parent
              21,
              'F',
              NULL,
              'N'
            )
    ```

## PHASE 2: CREATE JSON FOR THE VIEW
 - **STEP 1: Create JSON file**
   - File: `/config/view/{MODULO_ID}.json`
   - Note: You have to logout and login to update frontend changes

## PHASE 3: CREATE SERVICE
- **STEP 1:** Create router
   - File: `/services/{SERVICE_NAME}/index.js`
- **STEP 2:** Create service file
   - File: `/services/{SERVICE_NAME}/{SERVICE_NAME}.js`
- **STEP 3:** Create test file
   - File: `/services/{SERVICE_NAME}/{SERVICE_NAME}.spec.js`

## PHASE 4: CREATE ACTIONS (OPTIONAL)
- **STEP 1:** Create router for action
   - File: `/services/{SERVICE_NAME}/action/index.js`
- **STEP 2:** Create action file
   - File: `/services/{SERVICE_NAME}/action/{ACTION_NAME}.js`
