<template>
    <div>
        <table>
            <thead>
                <tr>
                    <th>Density of Dead Oil</th>
                    <th>Temperature</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>
                        <input type="text" v-model="density" placeholder="Density" /><br />
                        <small>4 digits after decimal</small>
                    </td>
                    <td>
                        <input type="text" v-model="temperature" placeholder="Temperature" /><br />
                        <small>ᵒ Celsius</small>
                    </td>
                </tr>
                <br />
                <tr>
                    <td colspan="2">
                        <button style="width: 100%" @click="calculate">Calculate</button>
                    </td>
                </tr>
            </tbody>
        </table>

        <p>Density Alpha : {{ density_alpha }}</p>
    </div>
</template>

<script>
import density_alpha_data from '~/assets/density_alpha.json'

export default {
    data() {
        return {
            density: 0.9079,
            temperature: 78,
            density_alpha: ''
        }
    },
    methods: {
        calculate() {
            this.alpha = 100

            let density = this.density.toString();

            let two_digits = density.slice(0, (density.indexOf(".")) + 3);

            let three_digits = density.slice(0, (density.indexOf(".")) + 4);

            let third_digit = density.slice(4, (density.indexOf(".")) + 4);

            let fourth_digit = density.slice(5, (density.indexOf(".")) + 5);

            let data = density_alpha_data.find(x => x.two_digits == two_digits && x.third_digit == third_digit)

            // Calculations
            let final_alpha = data.alpha_value + (fourth_digit * data.alpha)
            let delta_temperature = this.temperature - 15.6
            let multiplier = 1 + (final_alpha * delta_temperature * 0.0001)

            this.density_alpha = (three_digits / multiplier).toFixed(4)
        }
    }
}
</script>

<style lang="scss" scoped>
</style>