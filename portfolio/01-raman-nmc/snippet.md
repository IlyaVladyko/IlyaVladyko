# Essential Lines in Raman Kernel

```cpp
// each call generate a high-quality random float in range (0, 1) 
#define RandomNum rnd_generator(seeds)

// spontaneous Raman event
if (RandomNum < P_Raman) {
    photon.type = RAMAN;

    float theta = PI * RandomNum;
    float phi   = 2.0f * PI * RandomNum;

    photon.direction = isotropic_scattering(theta, phi);
}

// stimulated Raman interaction
if (photon.type != RAMAN) {
    for (neighbor : nearby_photons) {

        if (neighbor.type == RAMAN &&
            distance(photon, neighbor) < interaction_distance)
        {
            if (RandomNum < stim_prob) {
                photon.type = RAMAN;
                photon.direction = neighbor.direction; // inherit coherent direction
                break;
            }
        }
    }
}

// elastic scattering and absorption
if (photon.type != RAMAN && RandomNum < P_Elastic) {
    updatePhotonDirection(photon); // scattering with a given phase function
}
updatePhotonPosition(photon);

photon.absorb = photon.W * (1.0 - exp(-vdt / r_a));
photon.W -= photon.absorb;