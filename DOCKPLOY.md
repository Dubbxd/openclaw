# Deploy en Dockploy (virgen)

Guía rápida para desplegar este repo en un servidor Dockploy desde cero, sin secretos en Git.

## 1) Requisitos

- Dockploy funcionando
- Dominio apuntando al host (A/AAAA)
- Repo conectado a Dockploy (`main`)

## 2) Crear app en Dockploy

- **Provider:** GitHub
- **Repository:** `Dubbxd/openclaw`
- **Branch:** `main`
- **Build type:** Dockerfile
- **Dockerfile:** `Dockerfile`
- **Build path/context:** `/`

## 3) Variables de entorno mínimas

Usa estas en Dockploy (NO subas `.env` real al repo):

```env
NODE_ENV=production
PORT=8080
OPENCLAW_GATEWAY_PORT=8080
OPENCLAW_GATEWAY_BIND=lan
OPENCLAW_GATEWAY_TOKEN=REEMPLAZA_CON_TOKEN_NUEVO
OPENCLAW_STATE_DIR=/data/.openclaw
OPENCLAW_WORKSPACE_DIR=/data/workspace
TZ=America/Mexico_City
```

Opcional (si usarás OpenAI API):

```env
OPENAI_API_KEY=sk-...
```

Opcional (Telegram):

```env
TELEGRAM_BOT_TOKEN=123456:ABCDEF...
```

## 4) Persistencia (obligatorio)

Monta un volumen persistente en:

- **Container path:** `/data`

Si hay errores de permisos en `/data`, asigna ownership al UID/GID `1000:1000` en el host para la ruta del volumen.

## 5) Comando de arranque

Puedes dejar el default de la imagen. Si necesitas forzarlo:

```bash
node /app/openclaw.mjs gateway --allow-unconfigured --bind lan --port 8080
```

## 6) Dominio/TLS

- Asigna dominio (ej. `bot.tudominio.com`)
- Habilita HTTPS/Let's Encrypt
- Verifica:

```bash
curl -I https://bot.tudominio.com/
```

## 7) Primer acceso

- Dashboard: `https://TU_DOMINIO/`
- Setup wizard: `https://TU_DOMINIO/setup`
- Usa `OPENCLAW_GATEWAY_TOKEN` para conectar el dashboard.

## 8) Seguridad recomendada

- Activar BasicAuth en Dockploy/Traefik para el dominio
- Mantener token largo y rotarlo periódicamente
- Usar pairing/aprobación de dispositivos
- No exponer secretos en chat/capturas

## 9) Checklist final

- [ ] App en `1/1` réplicas
- [ ] `GET /` responde 200
- [ ] `/setup` accesible
- [ ] Volumen `/data` persistente
- [ ] Token gateway probado
- [ ] Secretos rotados/no expuestos
