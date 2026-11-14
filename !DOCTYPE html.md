<!DOCTYPE html>  
<html lang="de">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">  
    <title>Steal An Egg 3D .io</title>  
    <style>  
        * {  
            box-sizing: border-box;  
            user-select: none;  
            -webkit-user-select: none;  
            -webkit-touch-callout: none;  
            margin: 0;  
            padding: 0;  
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;  
        }  
  
        html, body {  
            width: 100%;  
            height: 100%;  
            overflow: hidden;  
            background-color: #1a1a2e;  
            touch-action: none;  
        }  
  
        #game-canvas {  
            display: block;  
            width: 100%;  
            height: 100%;  
            position: absolute;  
            top: 0;  
            left: 0;  
            z-index: 1;  
        }  
  
        /* STARTBILDSCHIRM - IPAD OPTIMIERT */  
        #start-screen {  
            position: fixed;  
            inset: 0;  
            background: radial-gradient(circle at center, rgba(30, 40, 80, 0.85), rgba(10, 12, 25, 0.96));  
            display: flex;  
            flex-direction: column;  
            align-items: center;  
            justify-content: center;  
            z-index: 9000;  
            pointer-events: auto;  
            backdrop-filter: blur(6px);  
        }  
  
        .title-logo {  
            font-size: clamp(2.5rem, 6vw, 5rem);  
            font-weight: 900;  
            color: #ffde59;  
            text-shadow: 0 6px 0 #d97706, 0 12px 25px rgba(0,0,0,0.6);  
            text-align: center;  
            letter-spacing: 2px;  
            margin-bottom: 10px;  
            transform: rotate(-2deg);  
        }  
  
        .subtitle {  
            color: #e2e8f0;  
            font-size: clamp(1rem, 2.2vw, 1.4rem);  
            margin-bottom: 35px;  
            text-align: center;  
            max-width: 600px;  
            padding: 0 20px;  
        }  
  
        #play-btn {  
            position: relative;  
            z-index: 9999 !important;  
            pointer-events: auto !important;  
            cursor: pointer;  
            background: linear-gradient(180deg, #4ade80 0%, #16a34a 100%);  
            color: white;  
            font-size: clamp(1.6rem, 3.5vw, 2.4rem);  
            font-weight: 900;  
            padding: 20px 55px;  
            border: 5px solid #ffffff;  
            border-radius: 24px;  
            box-shadow: 0 10px 0 #14532d, 0 18px 30px rgba(0,0,0,0.5);  
            transition: transform 0.1s ease;  
            touch-action: manipulation;  
        }  
  
        #play-btn:active {  
            transform: translateY(6px);  
            box-shadow: 0 4px 0 #14532d;  
        }  
  
        /* HUD & OVERLAYS */  
        #hud {  
            position: fixed;  
            inset: 0;  
            pointer-events: none;  
            z-index: 100;  
            display: none;  
        }  
  
        /* TOP LEFT: ADMIN + STATS */  
        .top-left-area {  
            position: absolute;  
            top: 15px;  
            left: 15px;  
            display: flex;  
            flex-direction: column;  
            gap: 10px;  
            pointer-events: none;  
        }  
  
        #admin-btn {  
            pointer-events: auto;  
            cursor: pointer;  
            background: linear-gradient(180deg, #ef4444, #b91c1c);  
            color: white;  
            font-weight: 900;  
            font-size: 1.1rem;  
            padding: 12px 22px;  
            border: 3px solid #fca5a5;  
            border-radius: 14px;  
            box-shadow: 0 5px 0 #7f1d1d;  
            width: fit-content;  
            touch-action: manipulation;  
        }  
  
        #admin-btn:active {  
            transform: translateY(3px);  
            box-shadow: 0 2px 0 #7f1d1d;  
        }  
  
        .stats-box {  
            background: rgba(15, 23, 42, 0.8);  
            border: 3px solid rgba(255, 255, 255, 0.25);  
            border-radius: 16px;  
            padding: 12px 18px;  
            color: white;  
            backdrop-filter: blur(4px);  
            min-width: 190px;  
        }  
  
        .stat-row {  
            font-size: 1.05rem;  
            font-weight: 800;  
            margin-bottom: 4px;  
            display: flex;  
            justify-content: space-between;  
            gap: 12px;  
        }  
  
        .stat-cash { color: #4ade80; }  
        .stat-income { color: #fde047; }  
        .stat-speed { color: #38bdf8; }  
  
        /* TOP CENTER: STATUS / NOTIFICATIONS */  
        #notification-banner {  
            position: absolute;  
            top: 15px;  
            left: 50%;  
            transform: translateX(-50%);  
            background: rgba(0, 0, 0, 0.82);  
            color: #fff;  
            padding: 10px 24px;  
            border-radius: 30px;  
            font-weight: 800;  
            font-size: 1.05rem;  
            border: 2px solid #facc15;  
            text-align: center;  
            opacity: 0;  
            transition: opacity 0.25s;  
            max-width: 90vw;  
        }  
  
        #carrying-indicator {  
            position: absolute;  
            top: 72px;  
            left: 50%;  
            transform: translateX(-50%);  
            background: linear-gradient(90deg, #f59e0b, #d97706);  
            color: #fff;  
            padding: 8px 20px;  
            border-radius: 20px;  
            font-weight: 900;  
            font-size: 1rem;  
            border: 2px solid #fff;  
            display: none;  
            box-shadow: 0 4px 12px rgba(0,0,0,0.4);  
        }  
  
        /* TOP RIGHT: LEADERBOARD */  
        #leaderboard {  
            position: absolute;  
            top: 15px;  
            right: 15px;  
            background: rgba(15, 23, 42, 0.82);  
            border: 3px solid rgba(255, 255, 255, 0.25);  
            border-radius: 16px;  
            padding: 12px 16px;  
            color: white;  
            width: 230px;  
            backdrop-filter: blur(4px);  
        }  
  
        #leaderboard h3 {  
            font-size: 0.95rem;  
            text-transform: uppercase;  
            letter-spacing: 1px;  
            color: #facc15;  
            margin-bottom: 8px;  
            border-bottom: 2px solid rgba(255,255,255,0.15);  
            padding-bottom: 4px;  
            text-align: center;  
        }  
  
        .lb-entry {  
            display: flex;  
            justify-content: space-between;  
            font-size: 0.9rem;  
            font-weight: 700;  
            padding: 3px 0;  
        }  
  
        .lb-me {  
            color: #4ade80;  
            font-weight: 900;  
        }  
  
        /* TOUCH CONTROLS (IMMER SICHTBAR IM SPIEL) */  
        #joystick-zone {  
            position: absolute;  
            bottom: 25px;  
            left: 25px;  
            width: 140px;  
            height: 140px;  
            background: rgba(255, 255, 255, 0.16);  
            border: 4px solid rgba(255, 255, 255, 0.45);  
            border-radius: 50%;  
            pointer-events: auto;  
            touch-action: none;  
            display: flex;  
            align-items: center;  
            justify-content: center;  
        }  
  
        #joystick-knob {  
            width: 60px;  
            height: 60px;  
            background: radial-gradient(circle at 30% 30%, #ffffff, #cbd5e1);  
            border-radius: 50%;  
            box-shadow: 0 5px 15px rgba(0,0,0,0.4);  
            position: absolute;  
            pointer-events: none;  
        }  
  
        .action-buttons {  
            position: absolute;  
            bottom: 25px;  
            right: 25px;  
            display: flex;  
            flex-direction: column;  
            align-items: flex-end;  
            gap: 14px;  
            pointer-events: none;  
        }  
  
        .action-row {  
            display: flex;  
            gap: 14px;  
            pointer-events: none;  
        }  
  
        .touch-btn {  
            pointer-events: auto;  
            touch-action: manipulation;  
            cursor: pointer;  
            border: 4px solid #ffffff;  
            color: white;  
            font-weight: 900;  
            font-size: 0.95rem;  
            border-radius: 22px;  
            box-shadow: 0 6px 0 rgba(0,0,0,0.45);  
            display: flex;  
            flex-direction: column;  
            align-items: center;  
            justify-content: center;  
            text-align: center;  
            transition: transform 0.08s;  
        }  
  
        .touch-btn:active {  
            transform: translateY(4px);  
            box-shadow: 0 2px 0 rgba(0,0,0,0.45);  
        }  
  
        #btn-jump {  
            width: 92px;  
            height: 92px;  
            border-radius: 50%;  
            background: linear-gradient(180deg, #38bdf8, #0284c7);  
        }  
  
        #btn-action {  
            width: 128px;  
            height: 82px;  
            background: linear-gradient(180deg, #facc15, #ca8a04);  
            color: #1e293b;  
        }  
  
        #btn-hit {  
            width: 96px;  
            height: 82px;  
            background: linear-gradient(180deg, #f87171, #dc2626);  
        }  
  
        /* CAMERA SWIPE ZONE RIGHT HALF */  
        #camera-touch-zone {  
            position: absolute;  
            top: 0;  
            right: 0;  
            width: 55%;  
            height: 75%;  
            pointer-events: auto;  
            z-index: 50;  
        }  
  
        /* ADMIN ABUSE MODAL */  
        #admin-modal {  
            position: fixed;  
            inset: 0;  
            background: rgba(0, 0, 0, 0.75);  
            z-index: 5000;  
            display: none;  
            align-items: center;  
            justify-content: center;  
            pointer-events: auto;  
            padding: 15px;  
        }  
  
        .admin-panel {  
            background: #1e293b;  
            border: 4px solid #ef4444;  
            border-radius: 24px;  
            width: 100%;  
            max-width: 500px;  
            padding: 22px;  
            color: white;  
            box-shadow: 0 20px 50px rgba(0,0,0,0.8);  
            max-height: 90vh;  
            overflow-y: auto;  
        }  
  
        .admin-header {  
            display: flex;  
            justify-content: space-between;  
