import React from "react";
import logo from "./logo.svg";
import "./App.css";
import "bootstrap/dist/css/bootstrap.min.css";
import Select from 'react-select';
import Swal from "sweetalert2";
import {
  Table,
  Button,
  Container,
  Modal,
  ModalHeader,
  ModalBody,
  FormGroup,
  ModalFooter,
} from "reactstrap";

const data = [

];

class App extends React.Component {
  state = {
    data: data,
    modalActualizar: false,
    modalInsertar: false,
    form: {
      id: "",
      nombre: "",
      apellidopaterno: "",
      apellidomaterno: "",
      curp: "",
      numero: "",
      domicilio: "",
      sexo: "",
      estadocivil: "",
    },
  };

  mostrarModalActualizar = (dato) => {
    this.setState({
      form: dato,
      modalActualizar: true,
    });
  };

  cerrarModalActualizar = () => {
    this.setState({ modalActualizar: false });
  };

  mostrarModalInsertar = () => {
    this.setState({
      modalInsertar: true,
    });
  };

  cerrarModalInsertar = () => {
    this.setState({ modalInsertar: false });
  };

  editar = (dato) => {
    var contador = 0;
    var arreglo = this.state.data;
    arreglo.map((registro) => {
      if (dato.id == registro.id) {
        arreglo[contador].nombre = dato.nombre;
        arreglo[contador].apellidopaterno = dato.apellidopaterno;
        arreglo[contador].apellidomaterno = dato.apellidomaterno;
        arreglo[contador].curp = dato.curp;
        arreglo[contador].numero = dato.numero;
        arreglo[contador].domicilio = dato.domicilio;
        arreglo[contador].sexo = dato.sexo;
        arreglo[contador].estadocivil = dato.estadocivil;

      }
      contador++;
    });
    this.setState({ data: arreglo, modalActualizar: false });
  };

  eliminar = (dato) => {
    var opcion = window.confirm("Estás Seguro que deseas Eliminar el elemento "+dato.id);
    if (opcion == true) {
      var contador = 0;
      var arreglo = this.state.data;
      arreglo.map((registro) => {
        if (dato.id == registro.id) {
          arreglo.splice(contador, 1);
        }
        contador++;
      });
      this.setState({ data: arreglo, modalActualizar: false });
    }
  };

  insertar= ()=>{
    var valorNuevo= {...this.state.form};
    valorNuevo.id=this.state.data.length+1;
    var lista= this.state.data;
    lista.push(valorNuevo);
    this.setState({ modalInsertar: false, data: lista });
  }

  handleChange = (e) => {
    this.setState({
      form: {
        ...this.state.form,
        [e.target.name]: e.target.value,
      },
    });
  };

  insertar = () => {
    // SweetAlert2 para confirmar la inserción
    Swal.fire({
      title: "¿Estás seguro?",
      text: "El elemento será insertado en la tabla",
      icon: "warning",
      showCancelButton: true,
      confirmButtonText: "Sí, agregar",
      cancelButtonText: "Cancelar",
    }).then((result) => {
      if (result.isConfirmed) {
        var valorNuevo = { ...this.state.form };
        valorNuevo.id = this.state.data.length + 1;
        var lista = this.state.data;
        lista.push(valorNuevo);
        this.setState({ modalInsertar: false, data: lista });
        // SweetAlert2 para confirmar la inserción exitosa
        Swal.fire(
          "¡Insertado!",
          "Registros guardados correctamente",
          "success"
        );
      }
    });
  };
  

  render() {
    
    return (
      <>
        <Container>
        <br />
          <Button color="success" onClick={()=>this.mostrarModalInsertar()}>Crear</Button>
          <br />
          <br />
          <Table>
            <thead>
              <tr>
                <th>ID</th>
                <th>Nombre</th>
                <th>Apellido Paterno</th>
                <th>Apellido Materno</th>
                <th>CURP</th>
                <th>Número de teléfono</th>
                <th>Domicilio</th>
                <th>Sexo</th>
                <th>Estado Civil</th>
              </tr>
            </thead>

            <tbody>
              {this.state.data.map((dato) => (
                <tr key={dato.id}>
                  <td>{dato.id}</td>
                  <td>{dato.nombre}</td>
                  <td>{dato.apellidopaterno}</td>
                  <td>{dato.apellidomaterno}</td>
                  <td>{dato.curp}</td>
                  <td>{dato.numero}</td>
                  <td>{dato.domicilio}</td>
                  <td>{dato.sexo}</td>
                  <td>{dato.estadocivil}</td>

                  <td>
                    <Button
                      color="primary"
                      onClick={() => this.mostrarModalActualizar(dato)}
                    >
                      Editar
                    </Button>{" "}
                    <Button color="danger" onClick={()=> this.eliminar(dato)}>Eliminar</Button>
                  </td>
                </tr>
              ))}
            </tbody>
          </Table>
        </Container>

        <Modal isOpen={this.state.modalActualizar}>
          <ModalHeader>
           <div><h3>Editar Registro</h3></div>
          </ModalHeader>

          <ModalBody>
            <FormGroup>
              <label>
               Id:
              </label>
            
              <input
                className="form-control"
                readOnly
                type="text"
                value={this.state.form.id}
              />
            </FormGroup>
            
            <FormGroup>
              <label>
                Nombre: 
              </label>
              <input
                className="form-control"
                name="nombre"
                type="text"
                onChange={this.handleChange}
                value={this.state.form.nombre}
              />
            </FormGroup>
            
            <FormGroup>
              <label>
                Apellido Paterno: 
              </label>
              <input
                className="form-control"
                name="apellidopaterno"
                type="text"
                onChange={this.handleChange}
                value={this.state.form.apellidopaterno}
              />
            </FormGroup>

            <FormGroup>
              <label>
                Apellido Materno: 
              </label>
              <input
                className="form-control"
                name="apellidomaterno"
                type="text"
                onChange={this.handleChange}
                value={this.state.form.apellidomaterno}
              />
            </FormGroup>
            <FormGroup>
              <label>
                CURP: 
              </label>
              <input
                className="form-control"
                name="curp"
                type="text"
                onChange={this.handleChange}
                value={this.state.form.curp}
              />
            </FormGroup>
            <FormGroup>
              <label>
                Número de teléfono: 
              </label>
              <input
                className="form-control"
                name="numero"
                type="text"
                onChange={this.handleChange}
                value={this.state.form.numero}
              />
            </FormGroup>
            <FormGroup>
              <label>
                Domicilio: 
              </label>
              <input
                className="form-control"
                name="domicilio"
                type="text"
                onChange={this.handleChange}
                value={this.state.form.domicilio}
              />
            </FormGroup>

            <FormGroup>
            <label>Género:</label>
  <Select
    options={[
      { value: 'masculino', label: 'Masculino' },
      { value: 'femenino', label: 'Femenino' },
    ]}
    value={{ value: this.state.form.sexo, label: this.state.form.sexo }}
    onChange={(selectedOption) => {
      this.setState({
        form: {
          ...this.state.form,
          sexo: selectedOption.value,
        },
      });
    }}
  />
            </FormGroup>

            
            <FormGroup>
            <label>Estado Civil:</label>
  <Select
    options={[
      { value: 'soltero/a', label: 'soltero/a' },
      { value: 'casado/a', label: 'casado/a' },
    ]}
    value={{ value: this.state.form.estadocivil, label: this.state.form.estadocivil }}
    onChange={(selectedOption) => {
      this.setState({
        form: {
          ...this.state.form,
          estadocivil: selectedOption.value,
        },
      });
    }}
  />
            </FormGroup>
          </ModalBody>

          <ModalFooter>
            <Button
              color="primary"
              onClick={() => this.editar(this.state.form)}
            >
              Editar
            </Button>
            <Button
              color="danger"
              onClick={() => this.cerrarModalActualizar()}
            >
              Cancelar
            </Button>
          </ModalFooter>
        </Modal>



        <Modal isOpen={this.state.modalInsertar}>
          <ModalHeader>
           <div><h3>Insertar Persona</h3></div>
          </ModalHeader>

          <ModalBody>
            <FormGroup>
              <label>
                Id: 
              </label>
              
              <input
                className="form-control"
                readOnly
                type="text"
                value={this.state.data.length+1}
              />
            </FormGroup>
            
            <FormGroup>
              <label>
                Nombre: 
              </label>
              <input
                className="form-control"
                name="nombre"
                type="text"
                onChange={this.handleChange}
              />
            </FormGroup>
            
            <FormGroup>
              <label>
                Apellido Paterno: 
              </label>
              <input
                className="form-control"
                name="apellidopaterno"
                type="text"
                onChange={this.handleChange}
              />
            </FormGroup>
            <FormGroup>
              <label>
                Apellido Materno: 
              </label>
              <input
                className="form-control"
                name="apellidomaterno"
                type="text"
                onChange={this.handleChange}
              />
            </FormGroup>
            <FormGroup>
              <label>
                CURP: 
              </label>
              <input
                className="form-control"
                name="curp"
                type="text"
                onChange={this.handleChange}
              />
            </FormGroup>
            <FormGroup>
              <label>
                Número de teléfono: 
              </label>
              <input
                className="form-control"
                name="numero"
                type="text"
                onChange={this.handleChange}
              />
            </FormGroup>

            <FormGroup>
              <label>
                Domicilio: 
              </label>
              <input
                className="form-control"
                name="domicilio"
                type="text"
                onChange={this.handleChange}
              />
            </FormGroup>

            <FormGroup>
            <label>Género:</label>
  <Select
    options={[
      { value: 'masculino', label: 'Masculino' },
      { value: 'femenino', label: 'Femenino' },
    ]}
    value={{ value: this.state.form.sexo, label: this.state.form.sexo }}
    onChange={(selectedOption) => {
      this.setState({
        form: {
          ...this.state.form,
          sexo: selectedOption.value,
        },
      });
    }}
  />
            </FormGroup>

            <FormGroup>
            <label>Estado Civil:</label>
  <Select
    options={[
      { value: 'soltero/a', label: 'soltero/a' },
      { value: 'casado/a', label: 'casado/a' },
    ]}
    value={{ value: this.state.form.estadocivil, label: this.state.form.estadocivil }}
    onChange={(selectedOption) => {
      this.setState({
        form: {
          ...this.state.form,
          estadocivil: selectedOption.value,
        },
      });
    }}
  />

            </FormGroup>


          </ModalBody>

          <ModalFooter>

            

            <Button
              color="primary"
              onClick={() => this.insertar()}
            >
              Insertar
            </Button>
            <Button
              className="btn btn-danger"
              onClick={() => this.cerrarModalInsertar()}
            >
              Cancelar
            </Button>
          </ModalFooter>
        </Modal>
      </>
    );
  }
}
export default App;
