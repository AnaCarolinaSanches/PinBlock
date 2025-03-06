import React from 'react';
import { View, Text, StyleSheet } from 'react-native';

const StepHorizontal = () => {
  return (
    <View style={styles.container}>
      <View style={styles.stepContainer}>
        {/* Passo 1 */}
        <View style={styles.step}>
          <Text style={styles.stepText}>1</Text>
        </View>
        {/* Passo 2 */}
        <View style={styles.step}>
          <Text style={styles.stepText}>2</Text>
        </View>
        {/* Passo 3 */}
        <View style={styles.step}>
          <Text style={styles.stepText}>3</Text>
        </View>
        {/* Passo 4 */}
        <View style={styles.step}>
          <Text style={styles.stepText}>4</Text>
        </View>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  stepContainer: {
    flexDirection: 'row',
    width: 328, // Largura total do container de passos
    height: 30, // Altura do container
    top: 17, // Distância de cima
    left: -126, // Distância à esquerda
    gap: 8, // Espaçamento entre os passos
  },
  step: {
    width: 30, // Largura de cada passo
    height: 30, // Altura de cada passo
    backgroundColor: '#ccc', // Cor de fundo do passo
    borderRadius: 15, // Deixa o passo arredondado
    justifyContent: 'center',
    alignItems: 'center',
  },
  stepText: {
    color: '#fff', // Cor do texto (número do passo)
    fontWeight: 'bold',
  }
});

export default StepHorizontal;
