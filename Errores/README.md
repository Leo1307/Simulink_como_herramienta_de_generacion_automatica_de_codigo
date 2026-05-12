# Error 5201

En caso de abrir matlab e inmediatamente se cierra, es posible que suceda por un error de comunicación, este error se demuestra en la siguiente figura.

[Error 5201]()

Para solventarlo, siga los siguientes pasos

## 1.Cierra MATLAB completamente

Ejecuta:
```bash
pkill -9 MATLAB
pkill -9 matlab
```

## 2.Borra caché y activaciones locales

MUY importante:

```bash
rm -rf ~/.MathWorks
rm -rf ~/.matlab
rm -rf ~/.config/MathWorks
```
## 3.Reinstala certificados por si MATLAB usa otros paths

```bash
sudo apt update
sudo apt install --reinstall ca-certificates
sudo update-ca-certificates
```

## 4.Verifica la fecha

```bash
timedatectl
```

Debe mostrar:

```bash
hora correcta
System clock synchronized: yes
```

Si no:

```bash
sudo timedatectl set-ntp true
```

## 5. Abrir matlab

```bash
matlab
```