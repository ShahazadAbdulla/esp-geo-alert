<script>
    import { Link } from "svelte-routing";
    import Nav from "./Nav.svelte";
    import HomeIcon from "./assets/house-chimney.svg";
    import Sensor from "./assets/sensor-on.svg";
    import { initializeApp } from "firebase/app";
    import { getDatabase, ref, onValue } from "firebase/database";

    const firebaseConfig = {
        apiKey: "AIzaSyCLbiKPaJ36j--ZGGa0kgqIdt-TZfpFIdI",
        authDomain: "loraalert-72e69.firebaseapp.com",
        databaseURL:
            "https://loraalert-72e69-default-rtdb.asia-southeast1.firebasedatabase.app",
        projectId: "loraalert-72e69",
        storageBucket: "loraalert-72e69.firebasestorage.app",
        messagingSenderId: "194512426008",
        appId: "1:194512426008:web:29571e0afcbaf8c5a91f6d",
        measurementId: "G-VYWDN4MEF1",
    };

    // Initialize state variables
    let accelerometerX = 0;
    let accelerometerY = 0;
    let accelerometerZ = 0;
    let pressure = 0;
    let altitude = 0;
    let moisture = 0;

    // Initialize Firebase
    const app = initializeApp(firebaseConfig);
    const database = getDatabase(app);

    // Reference to the sensors data
    const sensorsRef = ref(database, "geo_alert/sensors");

    // Listen for changes in the sensors data
    onValue(sensorsRef, (snapshot) => {
        try {
            const data = snapshot.val();
            if (data) {
                console.log("Received data:", data);
                accelerometerX = data.accel_x || 0;
                accelerometerY = data.accel_y || 0;
                accelerometerZ = data.accel_z || 0;
                pressure = data.pressure || 0;
                altitude = data.altitude || 0;
                moisture = data.moisture || 0;
            }
        } catch (error) {
            console.error("Error reading data:", error);
        }
    });
</script>

<main>
    <Nav />
    <div class="stats">
        <div class="stat">
            <div class="stat__title">
                <h1>Accelerometer <span>X Y Z</span></h1>
            </div>
            <div class="stat__tiles">
                <div class="stat__tile">
                    <h3>{accelerometerX.toFixed(2)}</h3>
                </div>
                <div class="stat__tile">
                    <h3>{accelerometerY.toFixed(2)}</h3>
                </div>
                <div class="stat__tile">
                    <h3>{accelerometerZ.toFixed(2)}</h3>
                </div>
            </div>
        </div>
        <div class="stat">
            <div class="stat__title">
                <h1>Pressure <span>mb</span></h1>
            </div>
            <div class="stat__tiles">
                <div class="stat__tile">
                    <h3>{pressure.toFixed(2)}</h3>
                </div>
            </div>
        </div>
        <div class="stat">
                    <div class="stat__title">
                        <h1>Moisture <span>g / m<sup>3</sup></span></h1>
                    </div>
                    <div class="stat__tiles">
                        <div class="stat__tile">
                            <h3>{moisture.toFixed(2)}</h3>
                        </div>
                    </div>
                </div><div class="stat">
            <div class="stat__title">
                <h1>Altitude <span>m</span></h1>
            </div>
            <div class="stat__tiles">
                <div class="stat__tile">
                    <h3>{altitude.toFixed(2)}</h3>
                </div>
            </div>
        </div>
    </div>
    <div class="foot">
        <Link to="/">
            <img src={HomeIcon} class="home" alt="sensor" />
        </Link>
        <img src={Sensor} class="super" alt="sensor" />
    </div>
</main>

<style>
    .stat {
        margin-top: 2rem;
    }
    .stat__title h1 {
        font-size: 0.75rem;
    }
    .stat__title h1 span {
        color: #00b894;
    }
    .stat__tiles {
        display: flex;
        justify-content: space-between;
        margin-top: 1rem;
    }
    .stat__tile {
        background: #111111aa;
        width: 30%;
        padding-block: 1rem;
    }
    .stat__tile h3 {
        font-size: 1rem;
        text-align: center;
    }
</style>
