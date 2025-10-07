# rocketseat
prompts
# Prompt Manager — Figma MCP

Este repositorio contiene una configuración mínima para usar el Figma Model Context Protocol (MCP) localmente.

El archivo `./.vscode/mcp.json` ya está preparado para leer el token de Figma desde la variable de entorno `FIGMA_TOKEN` (expansión en `cmd` con `%FIGMA_TOKEN%`).

## Qué hace
- Evita almacenar el token de Figma en el repositorio.
- Muestra cómo arranca el servidor MCP usando `figma-developer-mcp`.

## Requisitos
- Node.js y npm (para `npx`)
- Acceso a Figma y un Personal Access Token (PAT)
- Visual Studio Code (opcional, para usar la integración MCP/archivo `.vscode/mcp.json`)

## Obtener el token de Figma
1. Abre Figma → Account settings → Personal access tokens.
2. Crea un nuevo token y cópialo.

## Añadir el token a tu entorno
- Temporalmente en PowerShell (sólo para la sesión actual):

```powershell
$env:FIGMA_TOKEN = "TU_TOKEN_DE_FIGMA_AQUI"
```

- Permanente para el usuario (cerrar/abrir terminal o VS Code para aplicar):

```powershell
setx FIGMA_TOKEN "TU_TOKEN_DE_FIGMA_AQUI"
```

- En cmd.exe (temporal para esa sesión):

```cmd
set FIGMA_TOKEN=TU_TOKEN_DE_FIGMA_AQUI
```

Nota: `mcp.json` usa `cmd /c` y la variable se expande como `%FIGMA_TOKEN%`. Si prefieres ejecutar directamente desde PowerShell, usa la sintaxis `$env:FIGMA_TOKEN` al llamar a `npx`.

## Arrancar el MCP
- Desde Visual Studio Code (recomendado si tienes la extensión MCP/Model Context Protocol):
  - Usa la paleta (F1) y ejecuta `MCP: Start servers` o la acción equivalente que tengas configurada.

- Manual con PowerShell:

```powershell
npx -y figma-developer-mcp --figma-api-key=$env:FIGMA_TOKEN --stdio
```

- Manual con cmd.exe:

```cmd
npx -y figma-developer-mcp --figma-api-key=%FIGMA_TOKEN% --stdio
```

Observa la salida en la terminal: debería mostrar logs de inicialización y confirmación de conexión.

## Verificaciones y errores comunes
- 401/403: token inválido o sin permisos. Regenera el token en Figma.
- Problemas de red/proxy: verifica la conectividad y la configuración del proxy si aplica.
- Si el proceso no arranca, revisa que Node.js esté instalado (`node -v`) y que `npx` funcione.

## Seguridad y git
Si en algún momento el token fue añadido al repositorio, realiza estos pasos:

1. Rota/elimina el token en Figma (genera uno nuevo).
2. Quita el archivo que contiene secretos del index de git y añade una regla en `.gitignore`:

```powershell
git rm --cached .vscode/mcp.json
Add-Content .gitignore ".vscode/mcp.json"
git add .gitignore
git commit -m "Remove hardcoded figma token and ignore mcp.json"
```

3. Si necesitas eliminar el token de la historia de git, usa `git filter-repo` (recomendado) o `git filter-branch`. Esto re-escribe la historia y requiere cuidado; si lo deseas, puedo guiarte paso a paso.

## Nota sobre shells
- PowerShell usa `$env:FIGMA_TOKEN`; cmd usa `%FIGMA_TOKEN%`.
- `mcp.json` del proyecto está configurado para ejecutar `cmd /c npx ... --figma-api-key=%FIGMA_TOKEN% --stdio`.

## Siguientes pasos (opcionales)
- Ya he añadido un `.gitignore` para evitar subir `./.vscode/mcp.json` y un script de inicio para PowerShell en `scripts/start-mcp.ps1`.
- Puedo guiarte para eliminar el token de la historia de git si ya fue commiteado.

Si quieres que haga la limpieza del index de git por ti (quitar `mcp.json` del staging y commitear el cambio), dime y aplico los comandos necesarios.
